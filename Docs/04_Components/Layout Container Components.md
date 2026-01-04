# Layout Container Components 

Layout container components are used to build page structures and implement flexible layouts. They can only be used in **Section/Component** files (except Page which is used in App).

---

## 1. Block - Block-Level Container

### Introduction

- **Purpose**: Most basic block-level container with `display: block`
- **Use Cases**: Page structure foundation, content grouping, card wrapping, content sections
- **Characteristics**: Block-level element that fills full width, flexible width/height and spacing

### Key Properties


| Property           | Type | Description                                                                  |
| ------------------ | ---- | ---------------------------------------------------------------------------- |
| `show`             | BOOL | Show/hide component; false means not rendered (display: none). Default: true |
| All CSS properties | -    | Supports all standard CSS style properties                                   |

### Common StyleClasses


| StyleClass          | Description                                                   |
| ------------------- | ------------------------------------------------------------- |
| `CardDefault`       | Card default style (padding:24px, shadow, border-radius:12px) |
| `CardHoverable`     | Card with hover effects (elevates on hover, enhanced shadow)  |
| `CardInteractive`   | Card interactive style (clickable, visual feedback)           |
| `CardElevated`      | Elevated card (larger shadow)                                 |
| `CardFlat`          | Flat card (with border, no shadow)                            |
| `PanelDefault`      | Panel default style (padding:24px, border:1px)                |
| `PanelSecondary`    | Secondary panel (light background)                            |
| `Container`         | Container style                                               |
| `Section`           | Page section style                                            |
| `Well`              | Recessed box style                                            |
| `AlertInfo`         | Information alert box                                         |
| `AlertSuccess`      | Success alert box                                             |
| `AlertWarning`      | Warning alert box                                             |
| `AlertError`        | Error alert box                                               |
| `BlockCenter`       | Center aligned block                                          |
| `BlockSpaceBetween` | Space-between alignment                                       |

### VL Syntax Examples

**Example 1: User Card**

```vl
# Frontend Global Vars
$userInfo({name:STRING,email:STRING,avatar:STRING}) = {name:"John Doe",email:"john@example.com",avatar:"https://images.unsplash.com/..."}

# Frontend Tree
<Block-UserCard "userCard"> padding:"24px" background-color:"#ffffff" border-radius:"8px" box-shadow:"0 2px 8px rgba(0,0,0,0.1)"
-<Row-CardHeader> gap:"16px" align-items:"center"
--<Image-Avatar "avatar"> sourceUri:$userInfo.avatar width:"64px" height:"64px" border-radius:"50%"
--<Col-UserDetails>
---<Text-UserName> value:$userInfo.name StyleClass:HeadingSmall
---<Text-UserEmail> value:$userInfo.email StyleClass:TextSecondary
-<Divider-Separator> StyleClass:DividerLight
-<Row-Actions> gap:"12px"
--<Button-Edit "editBtn"> value:"Edit Profile" StyleClass:ButtonSecondary
--<Button-Settings "settingsBtn"> value:"Settings" StyleClass:ButtonText
```

**Example 2: Alert/Notification Box**

```vl
# Frontend Global Vars
$notificationMessage(STRING) = "Your order has been placed successfully!"
$notificationType(STRING) = "success"

# Frontend Tree
<Block-Notification "notif"> StyleClass:AlertSuccess padding:"16px" border-radius:"8px" margin-bottom:"16px"
-<Row-NotificationContent> gap:"12px" align-items:"flex-start"
--<Icon-AlertIcon> fontSet:"fa-solid-900" content:"f05a" font-size:"20px"
--<Col-NotificationMessage>
---<Text-Title> value:"Success" StyleClass:TextDefault font-weight:"500"
---<Text-Message> value:$notificationMessage StyleClass:TextSecondary
--<Button-Close "closeBtn"> value:"×" StyleClass:ButtonIcon cursor:"pointer"
```

**Example 3: Conditional Visibility**

```vl
# Frontend Global Vars
$isLoading(BOOL) = false
$data([{}]) = []
$error(STRING) = ""

# Frontend Tree
<If-Loading> conditions:$isLoading
-<Block-LoadingState "loading"> padding:"40px" text-align:"center"
--<Icon-Spinner> fontSet:"fa-solid-900" content:"f110" font-size:"32px" animation:"spin 1s linear infinite"
--<Text-LoadingMessage> value:"Loading..." StyleClass:TextSecondary margin-top:"16px"

<If-HasError> conditions:($error != "")
-<Block-ErrorState "error"> StyleClass:AlertError

--<Text-ErrorMessage> value:$error

<If-HasData> conditions:($data.length > 0)
-<Block-DataContent "content">
--<For-DataItems> sourceArray:$data loopVar:[_item0, _index0]
---<Block-DataItem> padding:"16px" border-bottom:"1px solid #e0e0e0"

<If-Empty> conditions:($data.length == 0 && !$isLoading && $error == "")
-<Block-EmptyState "empty"> padding:"40px" text-align:"center"
--<Icon-EmptyIcon> fontSet:"fa-solid-900" content:"f0c5" font-size:"48px" color:"#bfbfbf"
--<Text-EmptyMessage> value:"No data available" StyleClass:TextTertiary margin-top:"16px"
```

**Example 4: Card Grid**

```vl
# Frontend Global Vars
$cardList([{id:INT,title:STRING,description:STRING,image:STRING,price:FLOAT}]) = [
  {id:1,title:"Product A",description:"Great product",image:"https://images.unsplash.com/...",price:99.99},
  {id:2,title:"Product B",description:"Amazing deal",image:"https://images.unsplash.com/...",price:149.99}
]

# Frontend Tree
<Block-CardContainer> padding:"24px"
-<Row-CardGrid> gap:"16px" flex-wrap:"wrap"
--<For-Cards> sourceArray:$cardList loopVar:[_card0, _index0]
---<Block-Card "card"> width:"calc(25% - 12px)" StyleClass:CardHoverable
----<Image-CardImage> sourceUri:_card0.image width:"100%" height:"200px" object-fit:"cover"
----<Block-CardContent> padding:"16px"
-----<Text-CardTitle> value:_card0.title StyleClass:TextDefault
-----<Text-CardDescription> value:_card0.description StyleClass:TextSecondary
-----<Text-CardPrice> value:("$" + _card0.price) StyleClass:TextLarge color:"#0052D9" font-weight:"500"
-----<Button-AddCart "addBtn"> value:"Add to Cart" width:"100%" StyleClass:ButtonPrimary margin-top:"12px"
```

### Common Use Cases

```vl
# Use Case 1: Dashboard Widget
<Block-Widget "widget"> StyleClass:CardDefault width:"350px"
-<Block-WidgetHeader> padding-bottom:"16px" border-bottom:"1px solid #e0e0e0"
--<Text-WidgetTitle> value:"Revenue" StyleClass:HeadingSmall
--<Text-WidgetSubtitle> value:"Last 30 days" StyleClass:TextSecondary
-<Block-WidgetContent> padding-top:"16px"
--<Text-Amount> value:"$12,500" StyleClass:HeadingLarge
--<Text-Change> value:"+12% from last month" StyleClass:TextSuccess

# Use Case 2: Message List Item
<Block-MessageItem "item"> padding:"16px" border-bottom:"1px solid #f0f0f0" cursor:"pointer"
-<Row-MessageHeader> gap:"12px" align-items:"center" margin-bottom:"8px"
--<Text-SenderName> value:_message0.senderName StyleClass:TextDefault font-weight:"500"
--<Text-Time> value:_message0.time StyleClass:TextTertiary font-size:"12px"
-<Text-MessagePreview> value:_message0.preview StyleClass:TextSecondary

# Use Case 3: Status Banner
<Block-StatusBanner "banner"> StyleClass:AlertWarning padding:"12px 16px" border-radius:"6px" margin-bottom:"16px"
-<Row-StatusContent> gap:"12px" align-items:"center"
--<Icon-WarningIcon> fontSet:"fa-solid-900" content:"f071" font-size:"18px"
--<Col-StatusMessage> flex:"1"
---<Text-StatusTitle> value:"Attention Required" StyleClass:TextDefault
---<Text-StatusDetail> value:"Your session will expire in 5 minutes" StyleClass:TextSecondary font-size:"13px"
--<Button-Extend "extendBtn"> value:"Extend" StyleClass:ButtonText
```

---

## 2. Row - Row Container (Flex Row)

### Introduction

- **Purpose**: Flex row container with `display: flex` and `flex-direction: row`
- **Use Cases**: Navigation bars, toolbars, horizontal layouts, button groups, header sections
- **Characteristics**: Child elements arranged horizontally, easy spacing and alignment control

### Key Properties


| Property          | Type   | Description                                                                     |
| ----------------- | ------ | ------------------------------------------------------------------------------- |
| `gap`             | STRING | Space between children, e.g., "16px"                                            |
| `justify-content` | STRING | Horizontal alignment: flex-start, center, flex-end, space-between, space-around |
| `align-items`     | STRING | Vertical alignment: flex-start, center, flex-end, stretch                       |
| `flex-wrap`       | STRING | Wrapping behavior: nowrap (default), wrap, wrap-reverse                         |

### Common StyleClasses


| StyleClass    | Description                                             |
| ------------- | ------------------------------------------------------- |
| `RowDefault`  | Default row (align-items:center, gap:8px)               |
| `RowSpaced`   | Space-between alignment (justify-content:space-between) |
| `RowCentered` | Center alignment (justify-content:center)               |
| `RowStart`    | Left alignment (justify-content:flex-start)             |
| `RowEnd`      | Right alignment (justify-content:flex-end)              |
| `RowWrap`     | Auto-wrap (flex-wrap:wrap)                              |

### VL Syntax Examples

**Example 1: Navigation Bar**

```vl
# Frontend Global Vars
$currentUser({name:STRING,avatar:STRING}) = {name:"John Doe",avatar:"https://images.unsplash.com/..."}
$isMenuOpen(BOOL) = false

# Frontend Tree
<Row-Navbar "navbar"> gap:"24px" justify-content:"space-between" align-items:"center" padding:"0 24px" height:"64px" background-color:"#ffffff" border-bottom:"1px solid #e0e0e0"
-<Text-Logo> value:"MyApp" StyleClass:HeadingSmall color:"#0052D9" font-weight:"600"
-<Row-NavItems> gap:"32px" justify-content:"center" flex:"1"
--<Text-NavItem> value:"Home" StyleClass:TextDefault cursor:"pointer"
---<StateStyle-NavHover> trigger:hover color:"#0052D9"
--<Text-NavItem> value:"Products" StyleClass:TextDefault cursor:"pointer"
--<Text-NavItem> value:"About" StyleClass:TextDefault cursor:"pointer"
--<Text-NavItem> value:"Contact" StyleClass:TextDefault cursor:"pointer"
-<Row-UserSection> gap:"12px" align-items:"center"
--<Image-UserAvatar "avatar"> sourceUri:$currentUser.avatar width:"40px" height:"40px" border-radius:"50%" cursor:"pointer"
--<Button-UserMenu "userMenuBtn"> value:"Account" StyleClass:ButtonText
--<Button-Logout "logoutBtn"> value:"Logout" StyleClass:ButtonText
```

**Example 2: Toolbar**

```vl
# Frontend Global Vars
$selectedItems(INT) = 0

# Frontend Tree
<Row-Toolbar "toolbar"> gap:"12px" justify-content:"space-between" align-items:"center" padding:"12px 16px" background-color:"#f5f5f5" border-radius:"6px"
-<Row-LeftActions> gap:"12px" align-items:"center"
--<Button-Add "addBtn"> value:"Add" StyleClass:ButtonPrimary
--<Button-Edit "editBtn"> value:"Edit" StyleClass:ButtonSecondary
--<Button-Delete "deleteBtn"> value:"Delete" StyleClass:ButtonDanger
-<Row-RightActions> gap:"12px" align-items:"center"
--<Text-Counter> value:("Selected: " + $selectedItems) StyleClass:TextSecondary
--<Button-Refresh "refreshBtn"> value:"Refresh" StyleClass:ButtonIcon
--<Button-Settings "settingsBtn"> value:"⚙" StyleClass:ButtonIcon
```

**Example 3: Card Header**

```vl
# Frontend Global Vars
$cardTitle(STRING) = "Monthly Report"
$cardDate(STRING) = "January 2024"

# Frontend Tree
<Row-CardHeader "header"> gap:"16px" align-items:"center" justify-content:"space-between" padding-bottom:"16px" border-bottom:"1px solid #e0e0e0"
-<Col-HeaderInfo>
--<Text-CardTitle> value:$cardTitle StyleClass:HeadingSmall
--<Text-CardDate> value:$cardDate StyleClass:TextSecondary font-size:"12px"
-<Row-HeaderActions> gap:"8px" align-items:"center"
--<Button-DownloadBtn "downloadBtn"> value:"Download" StyleClass:ButtonIcon
--<Button-MoreBtn "moreBtn"> value:"..." StyleClass:ButtonIcon
```

**Example 4: Search Bar**

```vl
# Frontend Global Vars
$searchQuery(STRING) = ""
$searchResults([{}]) = []

# Frontend Tree
<Row-SearchBar "searchBar"> gap:"8px" width:"100%" max-width:"600px" margin:"0 auto"
-<Input-SearchInput "search"> value:$searchQuery placeholder:"Search products..." flex:"1" StyleClass:InputDefault padding:"10px 16px" border-radius:"24px"
-<Button-Search "searchBtn"> value:"Search" StyleClass:ButtonPrimary border-radius:"24px"
-<Button-Clear "clearBtn"> value:"Clear" StyleClass:ButtonText show:($searchQuery != "")

# Frontend Event Handlers

<Input-SearchInput>.@input(newValue)
-$searchQuery = newValue
-IF newValue == ""
--$searchResults = []
--RETURN
-searchProducts(newValue) -> _result
-$searchResults = _result.data

<Button-Search>.@click()
-searchProducts($searchQuery) -> _result
-$searchResults = _result.data

<Button-Clear>.@click()
-$searchQuery = ""
-$searchResults = []
```

**Example 5: Button Group with Wrap**

```vl
# Frontend Tree
<Row-TagContainer "tags"> gap:"8px" flex-wrap:"wrap" padding:"16px"
-<For-Tags> sourceArray:$tags loopVar:[_tag0, _index0]
--<Block-Tag "tag"> background-color:"#e9f0fe" color:"#0052D9" padding:"6px 12px" border-radius:"16px" cursor:"pointer"
---<Row-TagContent> gap:"8px" align-items:"center"
----<Text-TagLabel> value:_tag0.name StyleClass:TextSmall
----<Icon-TagClose> fontSet:"fa-solid-900" content:"f00d" font-size:"12px" cursor:"pointer"
```

### Common Use Cases

```vl
# Use Case 1: Pagination Controls
<Row-Pagination "pagination"> gap:"8px" justify-content:"center" align-items:"center" padding:"16px"
-<Button-PrevPage "prevBtn"> value:"← Previous" StyleClass:ButtonSecondary
-<For-PageNumbers> sourceArray:[1,2,3,4,5] loopVar:[_pageNum0, _index0]
--<Button-PageNum "pageBtn"> value:_--<Button-PageNum "pageBtn"> value:(_pageNum0 | STRING) StyleClass:($currentPage == _pageNum0 ? "ButtonPrimary" : "ButtonSecondary") cursor:"pointer"
-<Button-NextPage "nextBtn"> value:"Next →" StyleClass:ButtonSecondary

# Use Case 2: Filter Bar

<Row-FilterBar "filterBar"> gap:"12px" align-items:"center" padding:"16px" background-color:"#f5f5f5" border-radius:"8px" flex-wrap:"wrap"
-<Text-FilterLabel> value:"Filter by:" StyleClass:Label
-<SingleSelect_Dropdown "categoryFilter"> options:$categories value:$selectedCategory StyleClass:SelectDefault
-<SingleSelect_Dropdown "priceFilter"> options:$priceRanges value:$selectedPrice StyleClass:SelectDefault
-<SingleSelect_Dropdown "sortFilter"> options:$sortOptions value:$selectedSort StyleClass:SelectDefault
-<Button-ResetFilter "resetBtn"> value:"Reset" StyleClass:ButtonText

# Use Case 3: Breadcrumb Navigation

<Row-Breadcrumb "breadcrumb"> gap:"8px" align-items:"center" padding:"8px" font-size:"14px"
-<For-Breadcrumbs> sourceArray:$breadcrumbItems loopVar:[_item0, _index0]
--<If-NotLast> conditions:(_index0 < ($breadcrumbs.length - 1))
---<Text-BreadcrumbItem> value:_item0.label StyleClass:LinkDefault cursor:"pointer"
---<Text-Separator> value:"/" StyleClass:TextTertiary margin:"0 4px"
--<If-IsLast> conditions:(_index0 == ($breadcrumbItems.length - 1))
---<Text-BreadcrumbCurrent> value:_item0.label StyleClass:TextDefault font-weight:"500"

# Use Case 4: Rating Display

<Row-RatingStars "rating"> gap:"4px" align-items:"center"
-<For-Stars> sourceArray:[1,2,3,4,5] loopVar:[_star0, _index0]
--<Icon-Star "star"> fontSet:"fa-solid-900" content:"f005" font-size:"16px" color:(_index0 < $rating ? "#FAAD14" : "#d9d9d9") cursor:"pointer"
-<Text-RatingValue> value:("(" + $ratingCount + " reviews)") StyleClass:TextSecondary font-size:"12px"

```

---

## 3. Col - Column Container (Flex Column)

### Introduction
- **Purpose**: Flex column container with `display: flex` and `flex-direction: column`
- **Use Cases**: Forms, lists, sidebars, page layouts, vertical stacking
- **Characteristics**: Child elements arranged vertically, perfect for building vertical structures

### Key Properties
| Property | Type | Description |
|----------|------|-------------|
| `gap` | STRING | Space between children, e.g., "16px" |
| `justify-content` | STRING | Vertical alignment: flex-start, center, flex-end, space-between |
| `align-items` | STRING | Horizontal alignment: flex-start, center, flex-end, stretch |

### Common StyleClasses
| StyleClass | Description |
|-----------|-------------|
| `ColDefault` | Default column (gap:8px) |
| `ColCentered` | Horizontally centered (align-items:center) |
| `ColSpaced` | Space-between alignment (justify-content:space-between) |
| `ColStretch` | Stretch children (align-items:stretch) |

### VL Syntax Examples

**Example 1: Login Form**
```vl
# Frontend Global Vars
$email(STRING) = ""
$password(STRING) = ""
$isLoading(BOOL) = false
$rememberMe(BOOL) = false

# Frontend Derived Vars
$isFormValid(BOOL) = ($email.includes("@") && $password.length >= 6)

# Frontend Tree
<Col-LoginForm "form"> gap:"16px" width:"400px" padding:"32px" margin:"auto" StyleClass:CardDefault box-shadow:"0 4px 12px rgba(0,0,0,0.1)"
-<Block-FormHeader> text-align:"center" margin-bottom:"16px"
--<Text-FormTitle> value:"Welcome Back" StyleClass:HeadingLarge
--<Text-FormSubtitle> value:"Sign in to your account" StyleClass:TextSecondary margin-top:"8px"
-<Col-FormGroup> gap:"8px"
--<Text-Label> value:"Email Address" StyleClass:Label font-weight:"500"
--<Input-Email "emailInput"> value:$email type:"email" width:"100%" placeholder:"you@example.com" StyleClass:InputDefault
-<Col-FormGroup> gap:"8px"
--<Text-Label> value:"Password" StyleClass:Label font-weight:"500"
--<Input-Password "passwordInput"> value:$password type:"password" width:"100%" placeholder:"••••••••" StyleClass:InputDefault
-<Row-RememberMe> gap:"8px" align-items:"center"
--<BinaryToggle_Checkbox "rememberCheckbox"> checked:$rememberMe
--<Text-RememberLabel> value:"Remember me" StyleClass:TextDefault cursor:"pointer"
-<Button-SignIn "signInBtn"> value:($isLoading ? "Signing in..." : "Sign In") disabled:(!$isFormValid || $isLoading) width:"100%" StyleClass:ButtonPrimary
-<Row-SignUp> gap:"8px" justify-content:"center" margin-top:"8px"
--<Text-NoAccount> value:"Don't have an account?" StyleClass:TextSecondary
--<Text-SignUpLink> value:"Sign up" StyleClass:LinkDefault cursor:"pointer"

# Frontend Event Handlers

<Input-Email "emailInput">.@input(newValue)
-$email = newValue

<Input-Password "passwordInput">.@input(newValue)
-$password = newValue

<BinaryToggle_Checkbox "rememberCheckbox">.@onChange(checked)
-$rememberMe = checked

<Button-SignIn "signInBtn">.@click()
-$isLoading = true
-login($email, $password, $rememberMe) -> _result
-$isLoading = false
-IF _result.success
--<SysUI>.showToast("Login successful!", "success")
--<ClientUtils>.switchRoute("dashboard")
-ELSE
--<SysUI>.showToast(_result.message, "error")

<Text-SignUpLink>.@click()
-<ClientUtils>.switchRoute("signup")
```

**Example 2: Sidebar Menu**

```vl
# Frontend Global Vars
$menuItems([{id:INT,icon:STRING,label:STRING,route:STRING}]) = [
  {id:1,icon:"f015",label:"Dashboard",route:"dashboard"},
  {id:2,icon:"f07c",label:"Orders",route:"orders"},
  {id:3,icon:"f2c0",label:"Products",route:"products"},
  {id:4,icon:"f013",label:"Settings",route:"settings"}
]
$activeRoute(STRING) = "dashboard"

# Frontend Tree
<Col-Sidebar "sidebar"> gap:"8px" padding:"16px" background-color:"#f5f5f5" width:"240px" min-height:"100vh" overflow-y:"auto"
-<Block-SidebarHeader> padding:"16px 12px" margin-bottom:"16px"
--<Text-SidebarTitle> value:"MyApp" StyleClass:HeadingMedium color:"#0052D9"
-<For-MenuItems> sourceArray:$menuItems loopVar:[_item0, _index0]
--<Block-MenuItem "menuItem"> padding:"12px 16px" border-radius:"8px" cursor:"pointer" background-color:($activeRoute == _item0.route ? "#e9f0fe" : "transparent")
---<Row-MenuContent> gap:"12px" align-items:"center"
----<Icon-MenuIcon> fontSet:"fa-solid-900" content:_item0.icon font-size:"16px" color:($activeRoute == _item0.route ? "#0052D9" : "#595959")
----<Text-MenuLabel> value:_item0.label StyleClass:TextDefault color:($activeRoute == _item0.route ? "#0052D9" : "#595959") font-weight:($activeRoute == _item0.route ? "500" : "400")
---<StateStyle-MenuHover> trigger:hover background-color:"#f0f0f0"
-<Divider-SidebarDivider> StyleClass:DividerLight margin:"16px 0"
-<Block-SidebarFooter> padding:"16px 12px" border-top:"1px solid #e0e0e0"
--<Row-UserSection> gap:"12px" align-items:"center"
---<Image-UserAvatar> sourceUri:$currentUser.avatar width:"32px" height:"32px" border-radius:"50%"
---<Col-UserInfo> flex:"1"
----<Text-UserName> value:$currentUser.name StyleClass:TextSmall font-weight:"500"
----<Text-UserEmail> value:$currentUser.email StyleClass:TextSmall color:"#8c8c8c"

# Frontend Event Handlers

<Block-MenuItem "menuItem">.@click()
-$activeRoute = _item0.route
-<ClientUtils>.switchRoute(_item0.route)
```

**Example 3: Product Detail**

```vl
# Frontend Global Vars
$product({id:INT,title:STRING,price:FLOAT,rating:FLOAT,description:STRING,images:[STRING],inStock:BOOL}) = {id:1,title:"Premium Headphones",price:299.99,rating:4.5,description:"High-quality wireless headphones with active noise cancellation",images:["https://images.unsplash.com/..."],inStock:true}
$quantity(INT) = 1

# Frontend Tree
<Col-ProductDetail "detail"> gap:"24px" padding:"32px" max-width:"1000px" margin:"auto"
-<Row-ProductImages> gap:"16px"
--<Block-MainImage> flex:"1"
---<Image-ProductImage "mainImage"> sourceUri:$product.images[0] width:"100%" height:"400px" object-fit:"cover" border-radius:"8px"
--<Col-ThumbImages> gap:"8px" width:"80px"
---<For-Thumbnails> sourceArray:$product.images loopVar:[_image0, _index0]
----<Image-Thumbnail> sourceUri:_image0 width:"80px" height:"80px" border-radius:"6px" cursor:"pointer"
-<Col-ProductInfo> gap:"16px" flex:"1"
--<Text-ProductTitle> value:$product.title StyleClass:HeadingLarge
--<Row-RatingAndPrice> gap:"16px" align-items:"center"
---<Row-Rating> gap:"4px" align-items:"center"
----<For-Stars> sourceArray:[1,2,3,4,5] loopVar:[_star0, _index0]
-----<Icon-Star> fontSet:"fa-solid-900" content:"f005" font-size:"16px" color:(_index0 < ($product.rating | INT) ? "#FAAD14" : "#d9d9d9")
----<Text-RatingValue> value:(($product.rating | STRING) + " (1.2K reviews)") StyleClass:TextSecondary
---<Divider-PriceDivider> orientation:"vertical" height:"24px"
---<Text-Price> value:("$" + $product.price) StyleClass:HeadingMedium color:"#ff4d4f"
--<Text-Description> value:$product.description StyleClass:TextDefault line-height:"1.6"
--<Col-SelectOptions> gap:"12px"
---<Row-QuantityControl> gap:"12px" align-items:"center"
----<Text-QuantityLabel> value:"Quantity:" StyleClass:TextDefault
----<Button-QuantityMinus> value:"-" width:"40px" height:"40px" StyleClass:ButtonSecondary
----<Input-QuantityInput> value:($quantity | STRING) type:"number" width:"60px" text-align:"center" StyleClass:InputDefault
----<Button-QuantityPlus> value:"+" width:"40px" height:"40px" StyleClass:ButtonSecondary
--<Row-Actions> gap:"12px"
---<Button-AddCart "addCartBtn"> value:"Add to Cart" flex:"1" StyleClass:ButtonPrimary height:"48px" font-size:"16px"
---<Button-BuyNow "buyNowBtn"> value:"Buy Now" flex:"1" StyleClass:ButtonPrimary height:"48px" font-size:"16px"
---<Button-Wishlist "wishlistBtn"> value:"♥" width:"48px" height:"48px" StyleClass:ButtonSecondary
-<Divider-SectionDivider> StyleClass:DividerDefault
-<Col-ProductDetails> gap:"16px"
--<Text-DetailsTitle> value:"Product Details" StyleClass:HeadingSmall
--<Row-DetailItem> gap:"16px" padding:"12px 0" border-bottom:"1px solid #f0f0f0"
---<Text-DetailLabel> value:"SKU:" width:"100px" StyleClass:TextDefault font-weight:"500"
---<Text-DetailValue> value:("SKU-" + ($product.id | STRING)) StyleClass:TextDefault
--<Row-DetailItem> gap:"16px" padding:"12px 0" border-bottom:"1px solid #f0f0f0"
---<Text-DetailLabel> value:"Availability:" width:"100px" StyleClass:TextDefault font-weight:"500"
---<Text-DetailValue> value:($product.inStock ? "In Stock" : "Out of Stock") StyleClass:($product.inStock ? "TextSuccess" : "TextError")
--<Row-DetailItem> gap:"16px" padding:"12px 0"
---<Text-DetailLabel> value:"Shipping:" width:"100px" StyleClass:TextDefault font-weight:"500"
---<Text-DetailValue> value:"Free shipping on orders over $50" StyleClass:TextDefault
```

**Example 4: Checkout Form**

```vl
# Frontend Global Vars
$checkoutStep(INT) = 1
$shippingInfo({address:STRING,city:STRING,zipcode:STRING}) = {address:"",city:"",zipcode:""}
$paymentInfo({cardNumber:STRING,expiryDate:STRING,cvv:STRING}) = {cardNumber:"",expiryDate:"",cvv:""}

# Frontend Tree
<Col-CheckoutForm "checkout"> gap:"24px" padding:"32px" max-width:"600px" margin:"auto"
-<Block-CheckoutHeader>
--<Text-CheckoutTitle> value:"Checkout" StyleClass:HeadingLarge
--<Row-StepIndicator> gap:"12px" justify-content:"center" padding:"16px 0" border-bottom:"1px solid #e0e0e0"
---<Block-StepNumber> width:"40px" height:"40px" border-radius:"50%" background-color:($checkoutStep >= 1 ? "#0052D9" : "#f0f0f0") color:($checkoutStep >= 1 ? "#ffffff" : "#8c8c8c") display:"flex" align-items:"center" justify-content:"center" font-weight:"500"
----<Text-StepNum> value:"1"
---<Text-StepLabel> value:"Shipping" StyleClass:TextSmall color:($checkoutStep >= 1 ? "#0052D9" : "#8c8c8c")
---<Block-StepNumber> width:"40px" height:"40px" border-radius:"50%" background-color:($checkoutStep >= 2 ? "#0052D9" : "#f0f0f0") color:($checkoutStep >= 2 ? "#ffffff" : "#8c8c8c") display:"flex" align-items:"center" justify-content:"center" font-weight:"500"
----<Text-StepNum> value:"2"
---<Text-StepLabel> value:"Payment
