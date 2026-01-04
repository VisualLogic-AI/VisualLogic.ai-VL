# Extended UI Components

Extended UI components can only be used in **Section/Component** files. They provide specialized functionality for common UI patterns.

---

## 1. SingleSelect Group - Single Selection Components

### Purpose

Components for single-value selection from a list of options. User can select only one value at a time.

### Common Interface for All SingleSelect Components

**Key Properties**:


| Property      | Type                                   | Description                                               |
| ------------- | -------------------------------------- | --------------------------------------------------------- |
| `options`     | [{label:STRING, value:STRING\|NUMBER}] | Array of selectable options                               |
| `value`       | STRING\| NUMBER                        | Currently selected option value; null if nothing selected |
| `disabled`    | BOOL                                   | Disable selection if true                                 |
| `placeholder` | STRING                                 | Placeholder text (Dropdown and OverlayPicker only)        |

**Key Methods**:

- `focus()` - Make selector gain focus
- `blur()` - Make selector lose focus

**Key Events**:

- `@onChange(newValue)` - Fired when selected value changes
- `@onFocus()` - Fired when selector gains focus
- `@onBlur()` - Fired when selector loses focus
- `@onOpen()` - Fired when dropdown/overlay opens (Dropdown and OverlayPicker only)
- `@onClose()` - Fired when dropdown/overlay closes (Dropdown and OverlayPicker only)

### Sub-Components

#### 1.1 SingleSelect_Dropdown - Dropdown Single Select

- **Purpose**: PC-style dropdown menu for single selection
- **Use Cases**: Select from many options, standard form field, settings selection
- **Characteristics**: Space-efficient, mouse-friendly

**VL Example**:

```vl
# Frontend Global Vars
$selectedCountry(STRING) = "usa"
$countries([{label:STRING,value:STRING}]) = [
  {label:"United States",value:"usa"},
  {label:"United Kingdom",value:"uk"},
  {label:"Canada",value:"canada"},
  {label:"Australia",value:"australia"}
]

# Frontend Tree
<Col-CountrySelector> gap:"8px"
-<Text-Label> value:"Country" StyleClass:Label
-<SingleSelect_Dropdown "countryDropdown"> options:$countries value:$selectedCountry placeholder:"Select a country..."

# Frontend Event Handlers

<SingleSelect_Dropdown "countryDropdown">.@onChange(newValue)
-$selectedCountry = newValue
-loadCountryData(newValue) -> _result
```

#### 1.2 SingleSelect_RadioButtons - Radio Button Group

- **Purpose**: Display all options as radio buttons
- **Use Cases**: Few options (2-5), need visibility of all choices
- **Characteristics**: All options visible, easy comparison

**VL Example**:

```vl
# Frontend Global Vars
$shippingMethod(STRING) = "standard"
$shippingOptions([{label:STRING,value:STRING}]) = [
  {label:"Standard (5-7 days)",value:"standard"},
  {label:"Express (2-3 days)",value:"express"},
  {label:"Overnight",value:"overnight"}
]

# Frontend Tree
<Col-ShippingOptions> gap:"12px"
-<Text-Label> value:"Shipping Method" StyleClass:Label
-<SingleSelect_RadioButtons "shippingRadio"> options:$shippingOptions value:$shippingMethod

# Frontend Event Handlers

<SingleSelect_RadioButtons "shippingRadio">.@onChange(newValue)
-$shippingMethod = newValue
-calculateShippingCost($shippingMethod) -> _result
-$shippingCost = _result.cost
```

#### 1.3 SingleSelect_OverlayPicker - Modal Single Select

- **Purpose**: Single selection in modal overlay (touch-friendly)
- **Use Cases**: Mobile-friendly selection, large option lists
- **Characteristics**: Touch-optimized, can include search filtering

**VL Example**:

```vl
# Frontend Global Vars
$selectedCity(STRING) = ""
$cities([{label:STRING,value:STRING}]) = [...]

# Frontend Tree
<SingleSelect_OverlayPicker "cityPicker"> options:$cities value:$selectedCity placeholder:"Choose city..."

# Frontend Event Handlers

<SingleSelect_OverlayPicker "cityPicker">.@onChange(newValue)
-$selectedCity = newValue
```

---

## 2. MultiSelect Group - Multiple Selection Components

### Purpose

Components for selecting multiple values from a list. Returns selected values as an array.

### Common Interface for All MultiSelect Components

**Key Properties**:


| Property   | Type                                   | Description                                    |
| ---------- | -------------------------------------- | ---------------------------------------------- |
| `options`  | [{label:STRING, value:STRING\|NUMBER}] | Array of selectable options                    |
| `value`    | [STRING\| NUMBER]                      | Array of selected option values; default is [] |
| `readOnly` | BOOL                                   | User cannot modify value if true               |

**Key Methods**:

- `focus()` - Make selector gain focus
- `blur()` - Make selector lose focus

**Key Events**:

- `@onChange(newValues)` - Fired when selection changes, emits array of selected values
- `@onFocus()` - Fired when selector gains focus
- `@onBlur()` - Fired when selector loses focus
- `@onOpen()` - Fired when overlay opens (Dropdown and OverlayPicker only)
- `@onClose()` - Fired when overlay closes (Dropdown and OverlayPicker only)

### Sub-Components

#### 2.1 MultiSelect_Dropdown - Dropdown Multi Select

- **Purpose**: PC-style dropdown with checkboxes for multiple selection
- **Use Cases**: Select multiple items from many options, filter/tag selection
- **Characteristics**: Space-efficient, shows selected count

**VL Example**:

```vl
# Frontend Global Vars
$selectedTags([STRING]) = ["javascript", "react"]
$availableTags([{label:STRING,value:STRING}]) = [
  {label:"JavaScript",value:"javascript"},
  {label:"React",value:"react"},
  {label:"Vue",value:"vue"},
  {label:"Angular",value:"angular"},
  {label:"Node.js",value:"nodejs"}
]

# Frontend Tree
<Col-TagSelector> gap:"8px"
-<Text-Label> value:"Select Technologies" StyleClass:Label
-<MultiSelect_Dropdown "tagDropdown"> options:$availableTags value:$selectedTags placeholder:"Choose tags..."

# Frontend Event Handlers

<MultiSelect_Dropdown "tagDropdown">.@onChange(newValues)
-$selectedTags = newValues
-filterJobsByTags(newValues) -> _result
-$filteredJobs = _result.jobs
```

#### 2.2 MultiSelect_Checkboxes - Checkbox Group

- **Purpose**: Show all options as checkboxes
- **Use Cases**: Few options, need visibility of all choices, batch operations
- **Characteristics**: All options visible, manual selection/deselection

**VL Example**:

```vl
# Frontend Global Vars
$selectedPermissions([STRING]) = ["read", "create"]
$permissionOptions([{label:STRING,value:STRING}]) = [
  {label:"Read",value:"read"},
  {label:"Create",value:"create"},
  {label:"Edit",value:"edit"},
  {label:"Delete",value:"delete"},
  {label:"Admin",value:"admin"}
]

# Frontend Tree
<Col-PermissionSelector> gap:"12px"
-<Text-Label> value:"User Permissions" StyleClass:Label
-<MultiSelect_Checkboxes "permissionCheckboxes"> options:$permissionOptions value:$selectedPermissions

# Frontend Event Handlers

<MultiSelect_Checkboxes "permissionCheckboxes">.@onChange(newValues)
-$selectedPermissions = newValues
-updateUserPermissions($userId, newValues) -> _result
```

#### 2.3 MultiSelect_OverlayPicker - Modal Multi Select

- **Purpose**: Multi-selection in modal overlay (touch-friendly)
- **Use Cases**: Mobile selection, large option lists, multi-step selection
- **Characteristics**: Touch-optimized, modal-based selection

---

## 3. BinaryToggle Group - Two-State Toggle Components

### Purpose

Components for toggling between two states (true/false, on/off).

### Common Interface for All BinaryToggle Components

**Key Properties**:


| Property   | Type | Description                                                             |
| ---------- | ---- | ----------------------------------------------------------------------- |
| `checked`  | BOOL | Current state; true = on/checked, false = off/unchecked. Default: false |
| `readOnly` | BOOL | User cannot modify state if true                                        |

**Key Events**:

- `@onChange(newCheckedState)` - Fired when state changes
- `@onFocus()` - Fired when component gains focus
- `@onBlur()` - Fired when component loses focus

### Sub-Components

#### 3.1 BinaryToggle_Switch - Slide Toggle Switch

- **Purpose**: Classic on/off switch with sliding animation
- **Use Cases**: Feature toggles, settings switches, enable/disable options
- **Characteristics**: Smooth animation, intuitive toggle action

**VL Example**:

```vl
# Frontend Global Vars
$darkModeEnabled(BOOL) = false
$notificationsEnabled(BOOL) = true

# Frontend Tree
<Col-SettingsPanel> gap:"16px" padding:"24px"
-<Row-SettingItem> gap:"12px" align-items:"center" justify-content:"space-between"
--<Text-SettingLabel> value:"Dark Mode" StyleClass:TextDefault
--<BinaryToggle_Switch "darkModeSwitch"> checked:$darkModeEnabled
-<Row-SettingItem> gap:"12px" align-items:"center" justify-content:"space-between"
--<Text-SettingLabel> value:"Enable Notifications" StyleClass:TextDefault
--<BinaryToggle_Switch "notificationSwitch"> checked:$notificationsEnabled

# Frontend Event Handlers

<BinaryToggle_Switch "darkModeSwitch">.@onChange(newChecked)
-$darkModeEnabled = newChecked
-saveUserSetting("darkMode", newChecked) -> _result

<BinaryToggle_Switch "notificationSwitch">.@onChange(newChecked)
-$notificationsEnabled = newChecked
-updateNotificationSettings(newChecked) -> _result
```

#### 3.2 BinaryToggle_Checkbox - Single Checkbox

- **Purpose**: Single checkbox for on/off selection
- **Use Cases**: Accept terms, confirm actions, select single item
- **Characteristics**: Standard checkbox appearance, familiar to users

**VL Example**:

```vl
# Frontend Global Vars
$acceptTerms(BOOL) = false
$rememberMe(BOOL) = false

# Frontend Tree
<Col-LoginForm> gap:"16px" width:"400px"
-<Input-Email> placeholder:"Email"
-<Input-Password> placeholder:"Password"
-<Row-FormOptions> gap:"8px" align-items:"center"
--<BinaryToggle_Checkbox "rememberCheckbox"> checked:$rememberMe
--<Text-RememberLabel> value:"Remember me" StyleClass:TextDefault cursor:"pointer"
-<Row-TermsAccept> gap:"8px" align-items:"flex-start"
--<BinaryToggle_Checkbox "termsCheckbox"> checked:$acceptTerms
--<Text-TermsText> value:"I agree to the Terms of Service" StyleClass:TextDefault
-<Button-Login> value:"Login" disabled:(!$acceptTerms) StyleClass:ButtonPrimary

# Frontend Event Handlers

<BinaryToggle_Checkbox "rememberCheckbox">.@onChange(checked)
-$rememberMe = checked

<BinaryToggle_Checkbox "termsCheckbox">.@onChange(checked)
-$acceptTerms = checked
```

---

## 4. FileUploader Group - File Upload Components

### Purpose

Components for file upload with progress tracking and file management.

### Common Interface for All FileUploader Components

**Key Properties**:


| Property   | Type    | Description                                         |
| ---------- | ------- | --------------------------------------------------- |
| `value`    | [{...}] | Array of uploaded file info objects; default is []  |
| `multiple` | BOOL    | Allow multiple file uploads; default is false       |
| `accept`   | STRING  | Accepted file types, e.g., ".jpg,.png" or "image/*" |
| `maxSize`  | NUMBER  | Maximum file size in bytes                          |
| `readOnly` | BOOL    | User cannot modify value if true                    |

**Key Events**:

- `@onChange(newFiles)` - Fired when file list changes
- `@onUploadSuccess(fileInfo, response)` - Fired when upload succeeds
- `@onUploadError(fileInfo, error)` - Fired when upload fails
- `@onProgress(fileInfo, progressEvent)` - Progress event during upload

### Sub-Components

#### 4.1 FileUploader_Button - Button Upload

- **Purpose**: Click button to select and upload file
- **Use Cases**: Simple single/multiple file upload, document upload
- **Characteristics**: Minimal UI, standard file dialog

**VL Example**:

```vl
# Frontend Global Vars
$uploadedFiles([{name:STRING,size:INT,url:STRING}]) = []
$uploadProgress(INT) = 0

# Frontend Tree
<Col-FileUploadSection> gap:"12px"
-<Text-Label> value:"Upload Documents" StyleClass:Label
-<FileUploader_Button "docUploader"> multiple:true accept:".pdf,.doc,.docx" maxSize:(5242880)
-<If-HasFiles> conditions:($uploadedFiles.length > 0)
--<For-UploadedFiles> sourceArray:$uploadedFiles loopVar:[_file0, _index0]
---<Row-FileItem> gap:"12px" padding:"8px" border-bottom:"1px solid #e0e0e0"
----<Icon-FileIcon> fontSet:"fa-solid-900" content:"f15b" font-size:"16px"
----<Col-FileInfo> flex:"1"
-----<Text-FileName> value:_file0.name StyleClass:TextDefault
-----<Text-FileSize> value:((_file0.size / 1024) | STRING + "KB") StyleClass:TextSecondary font-size:"12px"
----<Button-Delete> value:"×" StyleClass:ButtonIcon

# Frontend Event Handlers

<FileUploader_Button "docUploader">.@onChange(newFiles)
-$uploadedFiles = newFiles

<FileUploader_Button "docUploader">.@onUploadSuccess(fileInfo, response)
-<SysUI>.showToast(fileInfo.name + " uploaded successfully", "success")

<FileUploader_Button "docUploader">.@onUploadError(fileInfo, error)
-<SysUI>.showToast("Failed to upload " + fileInfo.name, "error")

<Button-Delete>.@click()
-$uploadedFiles.splice(_index0, 1)
```

#### 4.2 FileUploader_Dropzone - Drag-Drop Upload Area

- **Purpose**: Drag and drop files into area to upload
- **Use Cases**: Bulk upload, intuitive upload experience, image galleries
- **Characteristics**: Drag-drop enabled, visual feedback on hover

**VL Example**:

```vl
# Frontend Global Vars
$draggedOverDropzone(BOOL) = false
$uploadedImages([{url:STRING,name:STRING}]) = []

# Frontend Tree
<Block-ImageUploadArea> padding:"40px" border:"2px dashed #d9d9d9" border-radius:"8px" text-align:"center" background-color:($draggedOverDropzone ? "#f0f7ff" : "transparent")
-<FileUploader_Dropzone "imageDropzone"> multiple:true accept:"image/*" maxSize:(10485760)
--<Icon-UploadIcon> fontSet:"fa-solid-900" content:"f093" font-size:"32px" color:"#1890ff"
--<Text-DropzoneText> value:"Drag and drop images here, or click to select" StyleClass:TextDefault margin-top:"12px"

# Frontend Event Handlers

<FileUploader_Dropzone "imageDropzone">.@onChange(newFiles)
-$uploadedImages = newFiles
```

#### 4.3 FileUploader_ImageList - Image List with Preview

- **Purpose**: Image upload with preview and deletion capabilities
- **Use Cases**: Product image management, profile picture upload, gallery management
- **Characteristics**: Built-in preview, delete button, organized list view

**VL Example**:vl---

## 5. Common StyleClasses for Extended Components

### SelectComponent Styles


| StyleClass      | Use Case                        |
| --------------- | ------------------------------- |
| `SelectDefault` | Default select/dropdown styling |
| `SelectError`   | Error state styling             |

### Input Relationship Styles

Extended components work seamlessly with validation patterns using derived variables for disabled states and error displays.

---

## Best Practices for Extended Components

### 1. **Proper Event Binding**

```vl
# ✅ CORRECT: Use @onChange for real-time updates
<SingleSelect_Dropdown "categoryFilter"> options:$categories value:$selectedCategory

<SingleSelect_Dropdown "categoryFilter">.@onChange(newValue)
-$selectedCategory = newValue
-filterProducts(newValue) -> _result
-$filteredProducts = _result.data

# ❌ WRONG: Trying to directly modify component
<SingleSelect_Dropdown "categoryFilter">.value = newValue
```
### 2. **Validation with Extended Components**

```vl
# Frontend Global Vars
$selectedRole(STRING) = ""
$formError(BOOL) = false

# Frontend Derived Vars
$isRoleSelected(BOOL) = ($selectedRole != "")

# Frontend Tree
<Col-RoleSelector> gap:"8px"
-<SingleSelect_Dropdown "roleSelect"> options:$roleOptions value:$selectedRole placeholder:"Select role..."
-<If-RoleError> conditions:($formError && !$isRoleSelected)
--<Text-RoleError> value:"Please select a role" StyleClass:TextError font-size:"12px"
-<Button-Submit> disabled:(!$isRoleSelected) StyleClass:ButtonPrimary
```
### 3. **Multi-Select with Tags Display**

```vl
# Frontend Global Vars
$selectedTags([STRING]) = []
$availableTags([{label:STRING,value:STRING}]) = [...]

# Frontend Tree
<Col-TagManager> gap:"12px"
-<MultiSelect_Dropdown "tagDropdown"> options:$availableTags value:$selectedTags
-<If-HasSelectedTags> conditions:($selectedTags.length > 0)
--<Row-SelectedTags> gap:"8px" flex-wrap:"wrap" margin-top:"8px"
---<For-SelectedTags> sourceArray:$selectedTags loopVar:[_tag0, _index0]
----<Block-Tag> background-color:"#e9f0fe" color:"#0052d9" padding:"6px 12px" border-radius:"16px"
-----<Row-TagContent> gap:"8px" align-items:"center"
------<Text-TagLabel> value:_tag0
------<Icon-TagRemove> fontSet:"fa-solid-900" content:"f00d" font-size:"12px" cursor:"pointer"

# Frontend Event Handlers

<Icon-TagRemove>.@click()
-_index(INT) = $selectedTags.indexOf(_tag0)
-$selectedTags.splice(_index, 1)
```
### 4. **File Upload Progress Tracking**

```vl
# Frontend Global Vars
$uploadProgress(INT) = 0
$isUploading(BOOL) = false

# Frontend Tree
<Col-FileUploadWithProgress> gap:"12px"
-<FileUploader_Button "fileUploader"> multiple:false accept:".pdf"
-<If-Uploading> conditions:$isUploading
--<Block-ProgressContainer> padding:"12px" background-color:"#f5f5f5" border-radius:"6px"
---<Row-ProgressHeader> justify-content:"space-between" margin-bottom:"8px"
----<Text-ProgressLabel> value:"Uploading..." StyleClass:TextDefault
----<Text-ProgressPercent> value:($uploadProgress + "%") StyleClass:TextDefault font-weight:"500"
---<Block-ProgressBar> width:"100%" height:"8px" background-color:"#e0e0e0" border-radius:"4px" overflow:"hidden"
----<Block-ProgressFill> width:(($uploadProgress + "%")) height:"100%" background-color:"#0052d9" transition:"width 0.3s ease"

# Frontend Event Handlers

<FileUploader_Button "fileUploader">.@onProgress(fileInfo, progressEvent)
-_percentage(INT) = (progressEvent.loaded / progressEvent.total * 100) | INT
-$uploadProgress = _percentage
-$isUploading = true

<FileUploader_Button "fileUploader">.@onUploadSuccess(fileInfo, response)
-$uploadProgress = 100
-$isUploading = false
-<SysUI>.showToast("Upload complete", "success")
```
### 5. **Cascading Selection Pattern**

```vl
# Frontend Global Vars
$selectedCountry(STRING) = ""
$selectedState(STRING) = ""
$states([{label:STRING,value:STRING}]) = []

# Frontend Tree
<Col-LocationSelector> gap:"16px"
-<SingleSelect_Dropdown "countrySelect"> options:$countries value:$selectedCountry placeholder:"Select country..."
-<If-CountrySelected> conditions:($selectedCountry != "")
--<SingleSelect_Dropdown "stateSelect"> options:$states value:$selectedState placeholder:"Select state..." disabled:($selectedCountry == "")
-<If-StateSelected> conditions:($selectedState != "")
--<Text-SelectedLocation> value:($selectedCountry + " - " + $selectedState) StyleClass:TextDefault

# Frontend Event Handlers

<SingleSelect_Dropdown "countrySelect">.@onChange(newValue)
-$selectedCountry = newValue
-$selectedState = ""
-loadStatesByCountry(newValue) -> _result
-$states = _result.states

<SingleSelect_Dropdown "stateSelect">.@onChange(newValue)
-$selectedState = newValue
```
---

## Summary Table: Extended Components Overview


| Component                      | Best For             | Key Feature                     |
| ------------------------------ | -------------------- | ------------------------------- |
| **SingleSelect_Dropdown**      | Standard selection   | Space-efficient, mouse-friendly |
| **SingleSelect_RadioButtons**  | Few options (2-5)    | All options visible             |
| **SingleSelect_OverlayPicker** | Mobile selection     | Touch-optimized, modal-based    |
| **MultiSelect_Dropdown**       | Multiple from many   | Checkboxes in dropdown          |
| **MultiSelect_Checkboxes**     | Few multi-selections | All options always visible      |
| **MultiSelect_OverlayPicker**  | Mobile multi-select  | Touch-optimized modal           |
| **BinaryToggle_Switch**        | On/Off settings      | Smooth toggle animation         |
| **BinaryToggle_Checkbox**      | Accept/confirm       | Standard checkbox               |
| **FileUploader_Button**        | Simple uploads       | Click-to-select                 |
| **FileUploader_Dropzone**      | Bulk uploads         | Drag-and-drop enabled           |
| **FileUploader_ImageList**     | Image management     | Built-in preview & delete       |

---

## Important Rules

### Selection Components

- Always use `@onChange` event to update bound variable
- Use derived variables for validation states
- Cannot directly modify component `value` property
- Support `disabled` property for conditional disabling

### File Upload Components

- File selection and upload happen automatically after file selection
- All operations (select, upload, delete) controlled via internal UI
- Use `@onChange`, `@onUploadSuccess`, `@onUploadError` events
- `value` property contains array of uploaded file info objects

### Data Flow

- **Unidirectional**: Component property → variable binding → event handler → variable update
- **No v-model**: All data updates must be explicit via event handlers
- **No direct manipulation**: Cannot directly call component methods to change state

