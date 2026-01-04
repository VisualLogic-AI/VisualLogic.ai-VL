# Logic Container Components

Logic container components are used to control rendering logic and handle data iteration. They have **no UI themselves**, **cannot have styles**, and **cannot have id properties**.

---

## 1. For - Loop Container (Section/Component Only)

### Introduction

- **Purpose**: Dynamically create repeated components based on array data source
- **Use Cases**:
  - Rendering lists and data tables
  - Creating repeating card layouts
  - Building dynamic galleries
  - Rendering search results
  - Creating menu item lists
  - Displaying repeating form fields
- **Characteristics**:
  - Pure logic container (no DOM element)
  - Children are rendered once for each array element
  - Loop variables available in child component properties and events

### Key Properties


| Property      | Type             | Description                                                                  |
| ------------- | ---------------- | ---------------------------------------------------------------------------- |
| `sourceArray` | ARRAY            | Array data source to iterate over (can be 1D, 2D, or object array)           |
| `loopVar`     | [STRING, STRING] | Array containing [loopItemVar, loopIndexVar] names, e.g.,`[_item0, _index0]` |

### Loop Variable Naming Conventions

- **Loop item variables**: Use descriptive names like `_user0`, `_product0`, `_message0` (for first level) or `_item0`, `_item1` (for generic iteration)
- **Index variables**: Use `_index0`, `_index1`, `_index2` (where N indicates nesting level starting from 0)
- **All must use underscore prefix** and camelCase naming
- **Nesting levels**: First level uses `_item0`/`_index0`, nested second level uses `_item1`/`_index1`, etc.

### Important Rules

- Loop variables are **only valid** in property bindings and event handlers of components **directly inside** the loop and their descendants
- Cannot reference loop variables outside the loop container
- Cannot reference loop variables in event handlers of components **not statically defined** inside the loop

### VL Syntax Examples

**Example 1: Simple Product List**

```vl
# Frontend Global Vars
$products([{id:INT,name:STRING,price:FLOAT,image:STRING}]) = [
  {id:1,name:"Laptop",price:999.99,image:"https://images.unsplash.com/..."},
  {id:2,name:"Mouse",price:29.99,image:"https://images.unsplash.com/..."},
  {id:3,name:"Keyboard",price:79.99,image:"https://images.unsplash.com/..."}
]
$selectedProductId(INT) = 0

# Frontend Tree
<Block-ProductContainer> padding:"24px"
-<For-ProductCards> sourceArray:$products loopVar:[_product0, _index0]
--<Block-ProductCard "card"> padding:"16px" border:"1px solid #e0e0e0" border-radius:"8px" cursor:"pointer"
---<Image-ProductImage> sourceUri:_product0.image width:"100%" height:"200px" object-fit:"cover" border-radius:"6px"
---<Text-ProductName> value:_product0.name StyleClass:TextDefault font-weight:"500" margin-top:"12px"
---<Text-ProductPrice> value:("$" + _product0.price) StyleClass:TextLarge color:"#ff4d4f" margin-top:"8px"
---<Button-ViewDetails "viewBtn"> value:"View Details" width:"100%" StyleClass:ButtonPrimary margin-top:"12px"

# Frontend Event Handlers

<Block-ProductCard "card">.@click()
-$selectedProductId = _product0.id
-<ClientUtils>.switchRoute("product/" + (_product0.id | STRING))

<Button-ViewDetails "viewBtn">.@click()
-$selectedProductId = _product0.id
-<ClientUtils>.switchRoute("product/" + (_product0.id | STRING))
```

**Example 2: User List with Actions**

```vl
# Frontend Global Vars
$users([{id:INT,name:STRING,email:STRING,status:STRING,avatar:STRING}]) = [
  {id:1,name:"Alice Johnson",email:"alice@example.com",status:"active",avatar:"https://images.unsplash.com/..."},
  {id:2,name:"Bob Smith",email:"bob@example.com",status:"inactive",avatar:"https://images.unsplash.com/..."},
  {id:3,name:"Carol White",email:"carol@example.com",status:"active",avatar:"https://images.unsplash.com/..."}
]
$selectedUserId(INT) = 0
$isDeleting(BOOL) = false

# Frontend Tree
<Block-UserListContainer> padding:"24px"
-<Table-UserTable> width:"100%" border-collapse:"collapse"
--<TableHeader-Header>
---<TableRow-HeaderRow> background-color:"#f5f5f5" font-weight:"500"
----<TableCell-HeaderId> value:"ID" width:"60px"
----<TableCell-HeaderName> value:"Name" flex:"1"
----<TableCell-HeaderEmail> value:"Email" flex:"1"
----<TableCell-HeaderStatus> value:"Status" width:"100px"
----<TableCell-HeaderActions> value:"Actions" width:"150px"
--<For-Users> sourceArray:$users loopVar:[_user0, _index0]
---<TableRow-UserRow> border-bottom:"1px solid #e0e0e0" hover-background:"#f5f5f5"
----<TableCell-Id> value:(_user0.id | STRING) text-align:"center"
----<TableCellContainer-Name>
-----<Row-UserInfo> gap:"12px" align-items:"center"
------<Image-Avatar> sourceUri:_user0.avatar width:"32px" height:"32px" border-radius:"50%"
------<Col-UserDetails>
-------<Text-UserName> value:_user0.name StyleClass:TextDefault
-------<Text-UserEmail> value:_user0.email StyleClass:TextSecondary font-size:"12px"
----<TableCell-Status> value:_user0.status
-----<StateStyle-ActiveStatus> conditions:(_user0.status == "active") color:"#52C41A"
-----<StateStyle-InactiveStatus> conditions:(_user0.status == "inactive") color:"#ff4d4f"
----<TableCellContainer-Actions>
-----<Row-ActionButtons> gap:"8px"
------<Button-Edit "editBtn"> value:"Edit" StyleClass:ButtonText font-size:"12px"
------<Button-Delete "deleteBtn"> value:"Delete" StyleClass:ButtonText color:"#ff4d4f" font-size:"12px"

# Frontend Event Handlers

<Button-Edit "editBtn">.@click()
-$selectedUserId = _user0.id
-<ClientUtils>.switchRoute("user/edit/" + (_user0.id | STRING))

<Button-Delete "deleteBtn">.@click()
-<SysUI>.showModal("Confirm Delete", ("Are you sure you want to delete " + _user0.name + "?")) -> _confirm
-IF _confirm.confirm
--$isDeleting = true
--deleteUser(_user0.id) -> _result
--$isDeleting = false
--IF _result.success
---$users.splice(_index0, 1)
---<SysUI>.showToast("User deleted successfully", "success")
--ELSE
---<SysUI>.showToast(_result.message, "error")
```

**Example 3: Nested Loops**

```vl
# Frontend Global Vars
$categories([{id:INT,name:STRING,products:[{id:INT,name:STRING,price:FLOAT}]}]) = [
  {id:1,name:"Electronics",products:[{id:101,name:"Laptop",price:999.99},{id:102,name:"Mouse",price:29.99}]},
  {id:2,name:"Books",products:[{id:201,name:"VL Guide",price:39.99},{id:202,name:"Web Dev",price:49.99}]}
]

# Frontend Tree
<Block-CategoriesContainer> padding:"24px"
-<For-Categories> sourceArray:$categories loopVar:[_category0, _index0]
--<Block-CategorySection> margin-bottom:"32px"
---<Text-CategoryTitle> value:_category0.name StyleClass:HeadingMedium margin-bottom:"16px"
---<Row-ProductsRow> gap:"16px" flex-wrap:"wrap"
----<For-Products> sourceArray:_category0.products loopVar:[_product1, _index1]
-----<Block-ProductCard> width:"calc(25% - 12px)" padding:"16px" StyleClass:CardDefault
------<Text-ProductName> value:_product1.name StyleClass:TextDefault
------<Text-ProductPrice> value:("$" + _product1.price) StyleClass:TextLarge color:"#0052D9"
------<Button-AddCart> value:"Add to Cart" width:"100%" StyleClass:ButtonPrimary margin-top:"12px"

# Frontend Event Handlers

<Button-AddCart>.@click()
-addToCart({categoryId:_category0.id,productId:_product1.id,price:_product1.price}) -> _result
-IF _result.success
--<SysUI>.showToast((_product1.name + " added to cart"), "success")
-ELSE
--<SysUI>.showToast(_result.message, "error")
```

**Example 4: Dynamic Form Fields**

```vl
# Frontend Global Vars
$formFields([{name:STRING,label:STRING,type:STRING,required:BOOL,options:[{label:STRING,value:STRING}]}]) = [
  {name:"firstName",label:"First Name",type:"text",required:true,options:[]},
  {name:"lastName",label:"Last Name",type:"text",required:true,options:[]},
  {name:"country",label:"Country",type:"select",required:true,options:[{label:"USA",value:"us"},{label:"UK",value:"uk"}]},
  {name:"agreeTerms",label:"I agree to terms",type:"checkbox",required:true,options:[]}
]
$formData({firstName:STRING,lastName:STRING,country:STRING,agreeTerms:BOOL}) = {firstName:"",lastName:"",country:"",agreeTerms:false}
$formErrors({firstName:STRING,lastName:STRING,country:STRING,agreeTerms:STRING}) = {firstName:"",lastName:"",country:"",agreeTerms:""}

# Frontend Tree
<Col-DynamicForm> gap:"16px" width:"500px" padding:"32px"
-<Text-FormTitle> value:"Dynamic Form" StyleClass:HeadingMedium margin-bottom:"16px"
-<For-FormFields> sourceArray:$formFields loopVar:[_field0, _index0]
--<Col-FormGroup> gap:"8px"
---<Text-FieldLabel> value:(_field0.label + (_field0.required ? " *" : "")) StyleClass:Label font-weight:"500"
---<If-TextInput> conditions:(_field0.type == "text")
----<Input-TextField> value:$formData[_field0.name] type:"text" width:"100%" placeholder:_field0.label StyleClass:InputDefault
---<If-SelectInput> conditions:(_field0.type == "select")
----<SingleSelect_Dropdown "selectField"> options:_field0.options value:$formData[_field0.name]
---<If-CheckboxInput> conditions:(_field0.type == "checkbox")
----<BinaryToggle_Checkbox "checkboxField"> checked:$formData[_field0.name]
---<If-HasError> conditions:($formErrors[_field0.name] != "")
----<Text-FieldError> value:$formErrors[_field0.name] StyleClass:TextError font-size:"12px"
-<Row-FormActions> gap:"12px" margin-top:"24px"
--<Button-Submit> value:"Submit" flex:"1" StyleClass:ButtonPrimary
--<Button-Reset> value:"Reset" flex:"1" StyleClass:ButtonSecondary

# Frontend Event Handlers

<Input-TextField>.@input(newValue)
-$formData[_field0.name] = newValue

<SingleSelect_Dropdown "selectField">.@onChange(newValue)
-$formData[_field0.name] = newValue

<BinaryToggle_Checkbox "checkboxField">.@onChange(checked)
-$formData[_field0.name] = checked

<Button-Submit>.@click()
-validateForm($formData) -> _validation
-$formErrors = _validation.errors
-IF _validation.success
--<SysUI>.showToast("Form submitted successfully", "success")
--submitForm($formData) -> _result
-ELSE
--<SysUI>.showToast("Please correct the errors", "error")

<Button-Reset>.@click()
-_keys([STRING]) = Object.keys($formData)
-FOR (_i(INT) = 0; _i < _keys.length; _i++)
--$formData[_keys[_i]] = ""
```

**Example 5: Search Results List**

```vl
# Frontend Global Vars
$searchQuery(STRING) = ""
$searchResults([{id:INT,title:STRING,description:STRING,url:STRING,date:TIMESTAMP}]) = []
$isSearching(BOOL) = false

# Frontend Tree
<Col-SearchResults> gap:"16px" padding:"24px"
-<Text-SearchTitle> value:("Search results for: " + $searchQuery) StyleClass:HeadingMedium
-<If-NoResults> conditions:($searchResults.length == 0 && !$isSearching)
--<Block-EmptyResults> padding:"40px" text-align:"center"
---<Icon-NoResults> fontSet:"fa-solid-900" content:"f002" font-size:"48px" color:"#bfbfbf"
---<Text-NoResultsMessage> value:"No results found" StyleClass:TextTertiary margin-top:"16px"
-<If-HasResults> conditions:($searchResults.length > 0)
--<For-Results> sourceArray:$searchResults loopVar:[_result0, _index0]
---<Block-ResultItem> padding:"16px" border-bottom:"1px solid #e0e0e0" cursor:"pointer"
----<Text-ResultTitle> value:_result0.title StyleClass:LinkDefault font-weight:"500" margin-bottom:"8px"
----<Text-ResultDescription> value:_result0.description StyleClass:TextSecondary line-height:"1.6" margin-bottom:"8px"
----<Row-ResultMeta> gap:"16px" font-size:"12px"
-----<Text-ResultUrl> value:_result0.url StyleClass:TextTertiary
-----<Text-ResultDate> value:_result0.date.format("MMM DD, YYYY") StyleClass:TextTertiary

# Frontend Event Handlers

<Block-ResultItem>.@click()
-<SysFile>.uploadImages(5, [_result0.url]) -> _download
-IF _download.success
--<SysUI>.showToast("Opening link...", "success")
```

---

## 2. If - Conditional Container

### Introduction

- **Purpose**: Conditionally render content based on boolean expressions
- **Use Cases**:
  - Show/hide content based on user state
  - Display different UI based on data availability
  - Show loading, success, or error states
  - Render different layouts based on business logic
  - Show/hide form fields based on other field values
  - Display permission-based content
- **Characteristics**:
  - Pure logic container (no DOM element)
  - Children rendered only when condition is true
  - No children rendered when condition is false
  - Condition evaluated on every render

### Key Properties


| Property     | Type | Description                                                                                                                    |
| ------------ | ---- | ------------------------------------------------------------------------------------------------------------------------------ |
| `conditions` | BOOL | Boolean expression that determines whether to render children. Can reference global vars, derived vars, loop vars, system vars |

### Important Rules

- **CRITICAL**: Forbidden to use for device type detection (e.g., `conditions:(SYSENV.deviceType == 'mobile')`)
- Must reflect **application business logic state** (user login status, data validity, form state, etc.)
- Cannot have `id` property
- Cannot have CSS styles

### VL Syntax
### VL Syntax Examples

**Example 1: Loading States**

```vl
# Frontend Global Vars
$isLoading(BOOL) = false
$hasError(BOOL) = false
$errorMessage(STRING) = ""
$data([{}]) = []

# Frontend Tree
<Block-DataContainer> padding:"24px"
-<If-Loading> conditions:$isLoading
--<Block-LoadingState> padding:"40px" text-align:"center"
---<Icon-Spinner> fontSet:"fa-solid-900" content:"f110" font-size:"32px"
---<Text-LoadingMessage> value:"Loading data..." StyleClass:TextSecondary margin-top:"16px"

-<If-Error> conditions:$hasError
--<Block-ErrorState> StyleClass:AlertError padding:"16px" border-radius:"8px"
---<Row-ErrorContent> gap:"12px" align-items:"flex-start"
----<Icon-ErrorIcon> fontSet:"fa-solid-900" content:"f057" font-size:"20px"
----<Col-ErrorMessage>
-----<Text-ErrorTitle> value:"Error" StyleClass:TextDefault font-weight:"500"
-----<Text-ErrorDetail> value:$errorMessage StyleClass:TextSecondary
----<Button-Retry> value:"Retry" StyleClass:ButtonText cursor:"pointer"

-<If-HasData> conditions:($data.length > 0 && !$isLoading && !$hasError)
--<For-DataItems> sourceArray:$data loopVar:[_item0, _index0]
---<Block-DataItem> padding:"16px" border-bottom:"1px solid #e0e0e0"

-<If-Empty> conditions:($data.length == 0 && !$isLoading && !$hasError)
--<Block-EmptyState> padding:"40px" text-align:"center"
---<Icon-EmptyIcon> fontSet:"fa-solid-900" content:"f0c5" font-size:"48px" color:"#bfbfbf"
---<Text-EmptyMessage> value:"No data available" StyleClass:TextTertiary

# Frontend Event Handlers

<Button-Retry>.@click()
-$hasError = false
-$isLoading = true
-loadData() -> _result
-$isLoading = false
-IF !_result.success
--$hasError = true
--$errorMessage = _result.message
--RETURN
-$data = _result.data
```

**Example 2: User Authentication States**

```vl
# Frontend Global Vars
$isLoggedIn(BOOL) = false
$userRole(STRING) = ""

# Frontend Tree
<Block-Navigation> padding:"16px" background-color:"#ffffff" border-bottom:"1px solid #e0e0e0"
-<Row-NavContent> gap:"16px" justify-content:"space-between" align-items:"center"
--<Text-AppName> value:"MyApp" StyleClass:HeadingSmall
--<Row-NavActions> gap:"12px" align-items:"center"
---<If-NotLoggedIn> conditions:(!$isLoggedIn)
----<Button-Login> value:"Login" StyleClass:ButtonPrimary
----<Button-SignUp> value:"Sign Up" StyleClass:ButtonSecondary
---<If-LoggedInAsUser> conditions:($isLoggedIn && $userRole == "user")
----<Button-Dashboard> value:"Dashboard" StyleClass:ButtonText
----<Button-Profile> value:"Profile" StyleClass:ButtonText
----<Button-Logout> value:"Logout" StyleClass:ButtonText
---<If-LoggedInAsAdmin> conditions:($isLoggedIn && $userRole == "admin")
----<Button-AdminDashboard> value:"Admin Dashboard" StyleClass:ButtonPrimary
----<Button-ManageUsers> value:"Manage Users" StyleClass:ButtonText
----<Button-SystemSettings> value:"Settings" StyleClass:ButtonText
----<Button-AdminLogout> value:"Logout" StyleClass:ButtonText

# Frontend Event Handlers

<Button-Login>.@click()
-<ClientUtils>.switchRoute("login")

<Button-SignUp>.@click()
-<ClientUtils>.switchRoute("signup")

<Button-Dashboard>.@click()
-<ClientUtils>.switchRoute("dashboard")

<Button-AdminDashboard>.@click()
-<ClientUtils>.switchRoute("admin/dashboard")

<Button-Logout>.@click()
-logout() -> _result
-IF _result.success
--$isLoggedIn = false
--$userRole = ""
--<SysUI>.showToast("Logged out successfully", "success")
--<ClientUtils>.switchRoute("home")
```

**Example 3: Form Validation States**

```vl
# Frontend Global Vars
$email(STRING) = ""
$password(STRING) = ""
$confirmPassword(STRING) = ""

# Frontend Derived Vars
$isEmailValid(BOOL) = ($email == "" || $email.includes("@"))
$isPasswordValid(BOOL) = ($password.length >= 8)
$isPasswordMatching(BOOL) = ($password == $confirmPassword)
$isFormValid(BOOL) = ($isEmailValid && $isPasswordValid && $isPasswordMatching)

# Frontend Tree
<Col-RegistrationForm> gap:"16px" width:"500px" padding:"32px"
-<Text-FormTitle> value:"Create Account" StyleClass:HeadingLarge

-<Col-EmailField> gap:"8px"
--<Text-EmailLabel> value:"Email Address" StyleClass:Label
--<Input-Email> value:$email type:"email" width:"100%" StyleClass:InputDefault
--<If-EmailError> conditions:(!$isEmailValid && $email != "")
---<Text-EmailError> value:"Please enter a valid email address" StyleClass:TextError font-size:"12px"
--<If-EmailValid> conditions:($isEmailValid && $email != "")
---<Row-EmailValidated> gap:"8px" align-items:"center"
----<Icon-CheckMark> fontSet:"fa-solid-900" content:"f058" font-size:"16px" color:"#52C41A"
----<Text-ValidatedMessage> value:"Email is valid" StyleClass:TextSuccess font-size:"12px"

-<Col-PasswordField> gap:"8px"
--<Text-PasswordLabel> value:"Password" StyleClass:Label
--<Input-Password> value:$password type:"password" width:"100%" StyleClass:InputDefault
--<If-PasswordTooShort> conditions:(!$isPasswordValid && $password != "")
---<Text-PasswordError> value:"Password must be at least 8 characters" StyleClass:TextError font-size:"12px"
--<If-PasswordValid> conditions:($isPasswordValid)
---<Row-PasswordValidated> gap:"8px" align-items:"center"
----<Icon-CheckMark> fontSet:"fa-solid-900" content:"f058" font-size:"16px" color:"#52C41A"
----<Text-ValidatedMessage> value:"Strong password" StyleClass:TextSuccess font-size:"12px"

-<Col-ConfirmPasswordField> gap:"8px"
--<Text-ConfirmLabel> value:"Confirm Password" StyleClass:Label
--<Input-ConfirmPassword> value:$confirmPassword type:"password" width:"100%" StyleClass:InputDefault
--<If-PasswordMismatch> conditions:(!$isPasswordMatching && $confirmPassword != "")
---<Text-MismatchError> value:"Passwords do not match" StyleClass:TextError font-size:"12px"
--<If-PasswordMatch> conditions:($isPasswordMatching && $confirmPassword != "")
---<Row-PasswordMatch> gap:"8px" align-items:"center"
----<Icon-CheckMark> fontSet:"fa-solid-900" content:"f058" font-size:"16px" color:"#52C41A"
----<Text-MatchedMessage> value:"Passwords match" StyleClass:TextSuccess font-size:"12px"

-<Button-Register> value:"Create Account" width:"100%" disabled:(!$isFormValid) StyleClass:ButtonPrimary margin-top:"16px"

# Frontend Event Handlers

<Input-Email>.@input(newValue)
-$email = newValue

<Input-Password>.@input(newValue)
-$password = newValue

<Input-ConfirmPassword>.@input(newValue)
-$confirmPassword = newValue

<Button-Register>.@click()
-registerUser($email, $password) -> _result
-IF _result.success
--<SysUI>.showToast("Account created successfully", "success")
--<ClientUtils>.switchRoute("login")
-ELSE
--<SysUI>.showToast(_result.message, "error")
```

**Example 4: Dynamic Content Display**

```vl
# Frontend Global Vars
$viewMode(STRING) = "grid"
$items([{id:INT,name:STRING,description:STRING}]) = []
$selectedItem({}) = {}
$showDetailPanel(BOOL) = false

# Frontend Tree
<Row-ToolbarRow> gap:"12px" padding:"16px" border-bottom:"1px solid #e0e0e0"
-<Button-GridView> value:"Grid View" disabled:($viewMode == "grid") StyleClass:ButtonIcon
-<Button-ListView> value:"List View" disabled:($viewMode == "list") StyleClass:ButtonIcon
-<Button-DetailView> value:"Detail View" disabled:($viewMode == "detail") StyleClass:ButtonIcon

-<If-GridView> conditions:($viewMode == "grid")
--<Row-GridContainer> gap:"16px" flex-wrap:"wrap" padding:"16px"
--<For-GridItems> sourceArray:$items loopVar:[_item0, _index0]
---<Block-GridCard> width:"calc(25% - 12px)" padding:"16px" StyleClass:CardHoverable cursor:"pointer"
----<Text-ItemName> value:_item0.name StyleClass:TextDefault
----<Text-ItemDescription> value:_item0.description StyleClass:TextSecondary font-size:"12px"

-<If-ListView> conditions:($viewMode == "list")
--<Col-ListContainer> padding:"16px"
--<For-ListItems> sourceArray:$items loopVar:[_item0, _index0]
---<Block-ListItem> padding:"12px" border-bottom:"1px solid #e0e0e0" cursor:"pointer"
----<Row-ItemRow> justify-content:"space-between" align-items:"center"
-----<Col-ItemInfo>
------<Text-ItemName> value:_item0.name StyleClass:TextDefault font-weight:"500"
------<Text-ItemDescription> value:_item0.description StyleClass:TextSecondary
-----<Icon-Arrow> fontSet:"fa-solid-900" content:"f054" font-size:"12px"

-<If-DetailView> conditions:($viewMode == "detail")
--<Row-DetailLayout> gap:"24px" padding:"16px"
---<Col-DetailList> flex:"0 0 300px" max-height:"500px" overflow-y:"auto"
----<For-DetailListItems> sourceArray:$items loopVar:[_item0, _index0]
-----<Block-DetailListItem> padding:"12px" border-radius:"6px" cursor:"pointer" background-color:($selectedItem.id == _item0.id ? "#e9f0fe" : "transparent")
------<Text-ItemNameInList> value:_item0.name StyleClass:TextDefault font-weight:($selectedItem.id == _item0.id ? "500" : "400")
---<Col-DetailPanel> flex:"1" padding:"16px" StyleClass:CardDefault
----<If-ItemSelected> conditions:($selectedItem.id > 0)
-----<Text-DetailTitle> value:$selectedItem.name StyleClass:HeadingMedium
-----<Text-DetailContent> value:$selectedItem.description StyleClass:TextDefault
----<If-NoItemSelected> conditions:($selectedItem.id == 0)
-----<Block-NoSelection> padding:"40px" text-align:"center"
------<Text-NoSelectionMessage> value:"Select an item to view details" StyleClass:TextTertiary

# Frontend Event Handlers

<Button-GridView>.@click()
-$viewMode = "grid"

<Button-ListView>.@click()
-$viewMode = "list"

<Button-DetailView>.@click()
-$viewMode = "detail"

<Block-GridCard "card">.@click()
-$selectedItem = _item0
-$showDetailPanel = true

<Block-ListItem>.@click()
-$selectedItem = _item0

<Block-DetailListItem>.@click()
-$selectedItem = _item0
```

**Example 5: Permission-Based UI**

```vl
# Frontend Global Vars
$userPermissions([STRING]) = ["read", "create"]
$canViewData(BOOL) = $userPermissions.includes("read")
$canEditData(BOOL) = $userPermissions.includes("write")
$canDeleteData(BOOL) = $userPermissions.includes("delete")
$canApproveData(BOOL) = $userPermissions.includes("approve")

# Frontend Tree
<Block-DataPanel> padding:"24px"
-<Text-Title> value:"Data Management" StyleClass:HeadingLarge margin-bottom:"16px"

-<If-CanViewData> conditions:$canViewData
--<Block-DataContent>
---<Text-Message> value:"You have permission to view data" StyleClass:TextDefault

-<If-CannotViewData> conditions:(!$canViewData)
--<Block-AccessDenied> StyleClass:AlertError padding:"16px"
---<Text-DeniedMessage> value:"You do not have permission to view this data" StyleClass:TextDefault

-<If-CanEdit> conditions:$canEditData
--<Row-EditActions> gap:"12px" margin-top:"16px"
---<Button-Edit> value:"Edit" StyleClass:ButtonPrimary
---<Button-Duplicate> value:"Duplicate" StyleClass:ButtonSecondary

-<If-CanDelete> conditions:$canDeleteData
--<Button-Delete> value:"Delete" StyleClass:ButtonDanger margin-top:"12px"

-<If-CanApprove> conditions:$canApproveData
--<Row-ApprovalActions> gap:"12px" margin-top:"16px"
---<Button-Approve> value:"Approve" StyleClass:ButtonSuccess
---<Button-Reject> value:"Reject" StyleClass:ButtonWarning

-<If-NoPermissions> conditions:(!$canViewData && !$canEditData && !$canDeleteData && !$canApproveData)
--<Block-NoPermissions> padding:"40px" text-align:"center" StyleClass:AlertWarning
---<Icon-LockIcon> fontSet:"fa-solid-900" content:"f023" font-size:"32px"
---<Text-NoPermissionsMessage> value:"You do not have access to any actions" StyleClass:TextDefault margin-top:"16px"
```

### Common Use Cases

```vl
# Use Case 1: Tab Content Switching
<If-TabContent1> conditions:($activeTab == "tab1")
-<Block-Tab1Content>

<If-TabContent2> conditions:($activeTab == "tab2")
-<Block-Tab2Content>

<If-TabContent3> conditions:($activeTab == "tab3")
-<Block-Tab3Content>

# Use Case 2: Status Display
<If-StatusPending> conditions:($orderStatus == "pending")
-<Text-PendingStatus> value:"Your order is pending" StyleClass:AlertWarning

<If-StatusProcessing> conditions:($orderStatus == "processing")
-<Text-ProcessingStatus> value:"Processing your order..." StyleClass:AlertInfo

<If-StatusCompleted> conditions:($orderStatus == "completed")
-<Text-CompletedStatus> value:"Order completed!" StyleClass:AlertSuccess

# Use Case 3: Search State Management
<If-SearchActive> conditions:($searchQuery != "" && $searchResults.length > 0)
-<Block-SearchResults>

<If-NoSearchResults> conditions:($searchQuery != "" && $searchResults.length == 0 && !$isSearching)
-<Block-NoResults> value:"No results found"

<If-Searching> conditions:$isSearching-<Block-SearchingState> value:"Searching..."

<If-SearchInactive> conditions:($searchQuery == "")
-<Block-SearchPrompt> value:"Enter a search term"

```

---

## 3. TreeFor - Tree Loop Container (Section/Component Only)

### Introduction
- **Purpose**: Handle hierarchical/tree-structured data with parent-child relationships
- **Use Cases**:
  - Tree menus and navigation structures
  - Category hierarchies with subcategories
  - Organization structures and org charts
  - File/folder browser structures
  - Comment threads and nested replies
  - Permission/role hierarchies
  - Expandable category lists
- **Characteristics**: 
  - Pure logic container (no DOM element)
  - Automatically manages tree expansion/collapse
  - Supports parent-child relationships via idField and pidField
  - Provides level information for styling different depths

### Key Properties
| Property | Type | Description |
|----------|------|-------------|
| `sourceArray` | ARRAY | Array of objects containing id and parent id fields for tree structure |
| `idField` | STRING | Field name for current node ID (typically the node's unique identifier) |
| `pidField` | STRING | Field name for parent node ID (parent cannot be 0; top-level nodes have empty value) |
| `loopVar` | [STRING, STRING, STRING, STRING, STRING] | Array containing [itemVar, indexVar, levelVar, expandedVar, levelIndexVar] |

### Loop Variables Explained
| Variable | Type | Description |
|----------|------|-------------|
| `_itemX` | OBJECT | Current node's data object |
| `_indexX` | INT | Current node's index in flattened array |
| `_levelX` | INT | Current node's depth level (top level = 0, each child = parent level + 1) |
| `_expandedX` | BOOL | Whether current node is expanded (true/false) |
| `_levelIndexX` | INT | Current node's index among its sibling nodes at same level |

### Key Methods
| Method | Description |
|--------|-------------|
| `expandAllNodes()` | Expands all expandable nodes in tree |
| `collapseAllNodes()` | Collapses all expanded nodes, keeping only top-level nodes visible |
| `expandOneNode(nodeIndex)` | Expands specific node by index |
| `collapseOneNode(nodeIndex)` | Collapses specific node by index |

### VL Syntax Examples

**Example 1: Category Menu**
```vl
# Frontend Global Vars
$categories([{id:INT,name:STRING,parentId:INT}]) = [
  {id:1,name:"Electronics",parentId:0},
  {id:2,name:"Computers",parentId:1},
  {id:3,name:"Laptops",parentId:2},
  {id:4,name:"Desktops",parentId:2},
  {id:5,name:"Phones",parentId:1},
  {id:6,name:"Accessories",parentId:1},
  {id:7,name:"Clothing",parentId:0},
  {id:8,name:"Men",parentId:7},
  {id:9,name:"Women",parentId:7}
]
$expandedNodes(INT) = -1

# Frontend Tree
<Col-CategoryMenu> gap:"0" width:"240px" padding:"16px"
-<Text-MenuTitle> value:"Categories" StyleClass:HeadingSmall margin-bottom:"16px"
-<TreeFor-Categories> sourceArray:$categories idField:"id" pidField:"parentId" loopVar:[_category0, _index0, _level0, _expanded0, _levelIndex0]
--<Block-CategoryItem> padding:("0 0 0 " + (_level0 * 16) + "px") margin-bottom:"4px"
---<Row-CategoryRow> gap:"8px" align-items:"center" padding:"8px 12px" border-radius:"6px" cursor:"pointer"
----<If-HasChildren> conditions:(_category0.id > 0)
-----<Icon-ExpandIcon> fontSet:"fa-solid-900" content:(_expanded0 ? "f078" : "f054") font-size:"12px" color:"#8c8c8c"
----<If-NoChildren> conditions:(_category0.id == 0)
-----<Block-IndentSpacer> width:"12px"
----<Text-CategoryName> value:_category0.name StyleClass:TextDefault flex:"1"
---<StateStyle-CategoryHover> trigger:hover background-color:"#f0f0f0"
---<StateStyle-CategoryActive> conditions:($selectedCategoryId == _category0.id) background-color:"#e9f0fe"

# Frontend Event Handlers

<Block-CategoryItem>.@click()
-$selectedCategoryId = _category0.id
-IF _category0.id > 0
--loadProductsByCategory(_category0.id) -> _result
```

**Example 2: File Browser**

```vl
# Frontend Global Vars
$fileStructure([{id:INT,name:STRING,type:STRING,parentId:INT,size:INT}]) = [
  {id:1,name:"Documents",type:"folder",parentId:0,size:0},
  {id:2,name:"Resume.pdf",type:"file",parentId:1,size:512},
  {id:3,name:"CoverLetter.doc",type:"file",parentId:1,size:256},
  {id:4,name:"Projects",type:"folder",parentId:1,size:0},
  {id:5,name:"Project A",type:"folder",parentId:4,size:0},
  {id:6,name:"main.js",type:"file",parentId:5,size:1024},
  {id:7,name:"style.css",type:"file",parentId:5,size:512},
  {id:8,name:"Images",type:"folder",parentId:0,size:0},
  {id:9,name:"photo1.jpg",type:"file",parentId:8,size:2048}
]
$selectedFileId(INT) = 0

# Frontend Tree
<Col-FileBrowser> gap:"0" width:"300px" padding:"16px" border-right:"1px solid #e0e0e0"
-<Text-BrowserTitle> value:"Files" StyleClass:HeadingSmall margin-bottom:"16px"
-<TreeFor-FileTree> sourceArray:$fileStructure idField:"id" pidField:"parentId" loopVar:[_file0, _index0, _level0, _expanded0, _levelIndex0]
--<Block-FileItem> padding:("0 0 0 " + (_level0 * 16) + "px") margin-bottom:"2px"
---<Row-FileRow> gap:"8px" align-items:"center" padding:"8px 12px" border-radius:"4px" cursor:"pointer" background-color:($selectedFileId == _file0.id ? "#e9f0fe" : "transparent")
----<If-FileIsFolder> conditions:(_file0.type == "folder")
-----<Icon-FolderIcon> fontSet:"fa-solid-900" content:(_expanded0 ? "f07c" : "f07b") font-size:"14px" color:"#faad14"
----<If-FileIsFile> conditions:(_file0.type == "file")
-----<Icon-FileIcon> fontSet:"fa-solid-900" content:"f15b" font-size:"14px" color:"#1890ff"
----<Text-FileName> value:_file0.name StyleClass:TextDefault flex:"1" font-size:"13px"
----<If-ShowFileSize> conditions:(_file0.type == "file")
-----<Text-FileSize> value:((_file0.size / 1024) | STRING + "KB") StyleClass:TextTertiary font-size:"11px"
---<StateStyle-FileHover> trigger:hover background-color:"#f5f5f5"

# Frontend Event Handlers

<Block-FileItem>.@click()
-$selectedFileId = _file0.id
-IF _file0.type == "file"
--openFile(_file0) -> _result
-ELSE IF _file0.type == "folder"
--IF _expanded0
---<TreeFor-FileTree>.collapseOneNode(_index0)
--ELSE
---<TreeFor-FileTree>.expandOneNode(_index0)
```

**Example 3: Organization Structure**

```vl
# Frontend Global Vars
$organization([{id:INT,name:STRING,position:STRING,parentId:INT,department:STRING}]) = [
  {id:1,name:"CEO",position:"Chief Executive Officer",parentId:0,department:"Executive"},
  {id:2,name:"CTO",position:"Chief Technology Officer",parentId:1,department:"Executive"},
  {id:3,name:"VP Engineering",position:"VP of Engineering",parentId:2,department:"Engineering"},
  {id:4,name:"Frontend Lead",position:"Frontend Team Lead",parentId:3,department:"Engineering"},
  {id:5,name:"Backend Lead",position:"Backend Team Lead",parentId:3,department:"Engineering"},
  {id:6,name:"Frontend Dev 1",position:"Frontend Developer",parentId:4,department:"Engineering"},
  {id:7,name:"Frontend Dev 2",position:"Frontend Developer",parentId:4,department:"Engineering"},
  {id:8,name:"Backend Dev 1",position:"Backend Developer",parentId:5,department:"Engineering"},
  {id:9,name:"CFO",position:"Chief Financial Officer",parentId:1,department:"Executive"},
  {id:10,name:"Accounting Manager",position:"Accounting Manager",parentId:9,department:"Finance"}
]

# Frontend Tree
<Col-OrgChart> gap:"16px" padding:"24px"
-<Text-OrgTitle> value:"Organization Structure" StyleClass:HeadingLarge
-<Row-OrgControls> gap:"12px" margin-bottom:"16px"
--<Button-ExpandAll> value:"Expand All" StyleClass:ButtonSecondary
--<Button-CollapseAll> value:"Collapse All" StyleClass:ButtonSecondary
-<Block-TreeContainer> padding:"16px" background-color:"#f5f5f5" border-radius:"8px"
--<TreeFor-OrgTree> sourceArray:$organization idField:"id" pidField:"parentId" loopVar:[_person0, _index0, _level0, _expanded0, _levelIndex0]
---<Block-OrgNode> padding:("0 0 0 " + (_level0 * 24) + "px") margin-bottom:"8px"
----<Row-NodeRow> gap:"8px" align-items:"flex-start" padding:"12px" background-color:"#ffffff" border-radius:"6px" border:"1px solid #e0e0e0"
-----<If-HasChildren> conditions:(_level0 == 0 || _level0 == 1 || _level0 == 2)
------<Icon-TreeIcon> fontSet:"fa-solid-900" content:(_expanded0 ? "f078" : "f054") font-size:"12px" color:"#0052d9" cursor:"pointer"
-----<If-NoChildren> conditions:(_level0 > 2)
------<Block-NoChildSpacer> width:"12px"
-----<Col-PersonInfo> flex:"1"
------<Text-PersonName> value:_person0.name StyleClass:TextDefault font-weight:"500"
------<Text-PersonPosition> value:_person0.position StyleClass:TextSecondary font-size:"12px"
------<Text-PersonDept> value:_person0.department StyleClass:TextTertiary font-size:"11px"
-----<StateStyle-NodeHover> trigger:hover background-color:"#f9f9f9" border-color:"#0052d9"

# Frontend Event Handlers

<Button-ExpandAll>.@click()
-<TreeFor-OrgTree "orgTree">.expandAllNodes()

<Button-CollapseAll>.@click()
-<TreeFor-OrgTree "orgTree">.collapseAllNodes()

<Icon-TreeIcon>.@click()
-IF _expanded0
--<TreeFor-OrgTree "orgTree">.collapseOneNode(_index0)
-ELSE
--<TreeFor-OrgTree "orgTree">.expandOneNode(_index0)
```

**Example 4: Comment Thread**

```vl
# Frontend Global Vars
$comments([{id:INT,author:STRING,content:STRING,timestamp:TIMESTAMP,parentId:INT,likes:INT,avatar:STRING}]) = [
  {id:1,author:"Alice",content:"Great article! Really helpful.",timestamp:SYSENV.currentTime,parentId:0,likes:5,avatar:"https://images.unsplash.com/..."},
  {id:2,author:"Bob",content:"I agree with Alice!",timestamp:SYSENV.currentTime,parentId:1,likes:2,avatar:"https://images.unsplash.com/..."},
  {id:3,author:"Carol",content:"Thanks for the feedback!",timestamp:SYSENV.currentTime,parentId:1,likes:1,avatar:"https://images.unsplash.com/..."},
  {id:4,author:"David",content:"I have a question...",timestamp:SYSENV.currentTime,parentId:0,likes:0,avatar:"https://images.unsplash.com/..."},
  {id:5,author:"Eve",content:"What do you mean?",timestamp:SYSENV.currentTime,parentId:4,likes:1,avatar:"https://images.unsplash.com/..."}
]

# Frontend Tree
<Col-CommentThread> gap:"16px" padding:"24px"
-<Text-CommentsTitle> value:"Comments" StyleClass:HeadingMedium
-<TreeFor-Comments> sourceArray:$comments idField:"id" pidField:"parentId" loopVar:[_comment0, _index0, _level0, _expanded0, _levelIndex0]
--<Block-CommentNode> padding:("0 0 0 " + (_level0 * 24) + "px") margin-bottom:"12px"
---<Row-CommentContent> gap:"12px"
----<Image-Avatar> sourceUri:_comment0.avatar width:"36px" height:"36px" border-radius:"50%"
----<Col-CommentBody> flex:"1"
-----<Block-CommentCard> padding:"12px" background-color:"#f5f5f5" border-radius:"6px"
------<Row-CommentHeader> gap:"12px" align-items:"center" margin-bottom:"8px" justify-content:"space-between"
-------<Row-AuthorInfo> gap:"8px" align-items:"center"
--------<Text-AuthorName> value:_comment0.author StyleClass:TextDefault font-weight:"500"
--------<Text-CommentTime> value:_comment0.timestamp.format("MMM DD, YYYY HH:mm") StyleClass:TextTertiary font-size:"12px"
-------<If-HasReplies> conditions:(_comment0.id > 0)
--------<Text-ReplyCount> value:("(" + "1" + " reply)") StyleClass:TextSecondary font-size:"12px" show:(_expanded0)
------<Text-CommentText> value:_comment0.content StyleClass:TextDefault
------<Row-CommentActions> gap:"12px" margin-top:"8px"
-------<Button-Like> value:("👍 " + (_comment0.likes | STRING)) StyleClass:ButtonText font-size:"12px"
-------<Button-Reply> value:"Reply" StyleClass:ButtonText font-size:"12px"
---<StateStyle-CommentHover> trigger:hover background-color:"#ffffff"

# Frontend Event Handlers

<Button-Like>.@click()
-likeComment(_comment0.id) -> _result
-IF _result.success
--_comment0.likes = _comment0.likes + 1
```

