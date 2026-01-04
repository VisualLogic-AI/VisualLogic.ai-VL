# Basic UI Components

## Basic UI Components Overview

Basic UI components can only be used in **Section/Component** files, NOT in App files.

---

## 1. Text - Text Display

**Purpose**: Display plain text content on the page.

**Use Cases**:

- Page titles and headings
- Description text and labels
- Status messages and notifications
- Content paragraphs

**Key Properties**:


| Property | Type   | Description             |
| -------- | ------ | ----------------------- |
| `value`  | STRING | Text content to display |

**Common StyleClasses**:

- `TextPrimary` - Default text style
- `TextSecondary` - Secondary/muted text
- `TextTertiary` - Tertiary text (lighter)
- `TextDisabled` - Disabled text appearance
- `HeadingXL`, `HeadingLarge`, `HeadingMedium`, `HeadingSmall` - Heading styles
- `Caption` - Small caption text
- `Label` - Form label text
- `TextLarge`, `TextSmall` - Size variants

**VL Syntax Example**:

```vl
# Frontend Global Vars
$productName(STRING) = "Premium Widget"
$quantity(INT) = 5

# Frontend Tree
<Block-ProductInfo>
-<Text-Title> value:"Product Details" StyleClass:HeadingLarge
-<Text-Name> value:$productName StyleClass:TextPrimary
-<Text-Quantity> value:("Quantity: " + $quantity) StyleClass:TextSecondary
-<Text-Status> value:($quantity > 0 ? "In Stock" : "Out of Stock") StyleClass:TextTertiary
```

---

## 2. Button - Interactive Button

**Purpose**: Trigger user actions through clickable buttons.

**Use Cases**:

- Submit forms
- Navigate between pages
- Confirm/cancel dialogs
- Trigger data operations (save, delete, etc.)
- Call backend services

**Key Properties**:


| Property   | Type   | Description            |
| ---------- | ------ | ---------------------- |
| `value`    | STRING | Button label text      |
| `disabled` | BOOL   | Disable button if true |

**Common StyleClasses**:

- `ButtonPrimary` - Primary action (main button)
- `ButtonSecondary` - Secondary action
- `ButtonDanger` - Destructive action (delete, cancel)
- `ButtonSuccess` - Positive/confirm action
- `ButtonWarning` - Warning/caution action
- `ButtonText` - Text-only button (no background)
- `ButtonGhost` - Ghost/outline button
- `ButtonIcon` - Icon-only button
- `ButtonLink` - Link-style button
- `ButtonLarge`, `ButtonSmall` - Size variants

**Common Events**:

- `@click()` - Button clicked

**VL Syntax Example**:

```vl
# Frontend Global Vars
$isLoading(BOOL) = false
$canSubmit(BOOL) = true

# Frontend Tree
<Block-Actions>
-<Button-Submit> value:"Save Changes" disabled:(!$canSubmit) StyleClass:ButtonPrimary
-<Button-Cancel> value:"Cancel" StyleClass:ButtonSecondary
-<Button-Delete> value:"Delete" disabled:$isLoading StyleClass:ButtonDanger
-<Button-Help> value:"?" StyleClass:ButtonIcon

# Frontend Event Handlers

<Button-Submit>.@click()
-IF !$canSubmit
--<SysUI>.showToast("Please complete all required fields", "warning")
--RETURN
-$isLoading = true
-saveData($formData) -> _result
-$isLoading = false
-IF _result.success
--<SysUI>.showToast("Changes saved successfully", "success")
--<ClientUtils>.switchRoute("home")
-ELSE
--<SysUI>.showToast(_result.message, "error")

<Button-Cancel>.@click()
-<ClientUtils>.switchRoute("previous")

<Button-Delete>.@click()
-<SysUI>.showModal("Confirm Delete", "Are you sure you want to delete this item?") -> _confirm
-IF _confirm.confirm
--deleteItem($itemId) -> _result
--<SysUI>.showToast("Item deleted", "success")
```

---

## 3. Input - Single Line Text Input

**Purpose**: Collect single-line text input from users.

**Use Cases**:

- Username/email input
- Search field
- Form text fields
- Configuration settings
- Phone numbers, URLs, etc.

**Key Properties**:


| Property   | Type   | Description                                         |
| ---------- | ------ | --------------------------------------------------- |
| `value`    | STRING | Input field content (initial value)                 |
| `type`     | STRING | Input type: text, email, password, number, url, tel |
| `disabled` | BOOL   | Disable input if true                               |

**Common StyleClasses**:

- `InputDefault` - Default input style
- `InputError` - Error state
- `InputSuccess` - Success state
- `InputLarge`, `InputSmall` - Size variants

**Common Events**:

- `@change(newValue, oldValue)` - Input loses focus and value changed
- `@input(newValue)` - Input value changes (real-time)
- `@focus()` - Input gains focus
- `@blur()` - Input loses focus
- `@clickOutside()` - Click outside input area

**VL Syntax Example**:

```vl
# Frontend Global Vars
$email(STRING) = ""
$password(STRING) = ""
$formErrors({email:STRING,password:STRING}) = {email:"",password:""}

# Frontend Derived Vars
$isEmailValid(BOOL) = ($email == "" || $email.includes("@"))
$isPasswordValid(BOOL) = ($password.length >= 6)
$canLogin(BOOL) = ($isEmailValid && $isPasswordValid)

# Frontend Tree
<Block-LoginForm>
-<Block-EmailField>
--<Text-EmailLabel> value:"Email Address" StyleClass:Label
--<Input-Email> value:$email type:"email" StyleClass:InputDefault
--<If-EmailError> conditions:(!$isEmailValid && $email != "")
---<Text-EmailError> value:"Please enter a valid email" StyleClass:TextError
-<Block-PasswordField>
--<Text-PasswordLabel> value:"Password" StyleClass:Label
--<Input-Password> value:$password type:"password" StyleClass:InputDefault
--<If-PasswordError> conditions:(!$isPasswordValid && $password != "")
---<Text-PasswordError> value:"Password must be at least 6 characters" StyleClass:TextError
-<Button-Login> value:"Login" disabled:(!$canLogin) StyleClass:ButtonPrimary

# Frontend Event Handlers

<Input-Email>.@input(newValue)
-$email = newValue

<Input-Password>.@input(newValue)
-$password = newValue

<Button-Login>.@click()
-login($email, $password) -> _result
-IF _result.success
--<SysUI>.showToast("Login successful", "success")
--<ClientUtils>.switchRoute("dashboard")
-ELSE
--<SysUI>.showToast(_result.message, "error")
```

---

## 4. Textarea - Multi-Line Text Input

**Purpose**: Collect multi-line text input from users.

**Use Cases**:

- Comments and feedback
- Description fields
- Message/note input
- Article/content editing
- Long-form text input

**Key Properties**:


| Property   | Type   | Description                      |
| ---------- | ------ | -------------------------------- |
| `value`    | STRING | Textarea content (initial value) |
| `disabled` | BOOL   | Disable textarea if true         |

**Common StyleClasses**:

- `TextareaDefault` - Default textarea style
- `TextareaError` - Error state

**Common Events**:

- `@change(newValue, oldValue)` - Content changes after blur
- `@input(newValue)` - Real-time input changes
- `@focus()` - Textarea gains focus
- `@blur()` - Textarea loses focus

**VL Syntax Example**:

```vl
# Frontend Global Vars
$feedback(STRING) = ""
$characterCount(INT) = 0

# Frontend Tree
<Block-FeedbackForm>
-<Text-Title> value:"Send us your feedback" StyleClass:HeadingMedium
-<Textarea-Feedback> value:$feedback StyleClass:TextareaDefault
-<Text-Counter> value:("Characters: " + $characterCount + "/500") StyleClass:TextSecondary
-<Button-Submit> value:"Submit Feedback" StyleClass:ButtonPrimary

# Frontend Event Handlers

<Textarea-Feedback>.@input(newValue)
-$feedback = newValue
-$characterCount = newValue.length

<Button-Submit>.@click()
-IF $feedback.length < 10
--<SysUI>.showToast("Please enter at least 10 characters", "warning")
--RETURN
-submitFeedback($feedback) -> _result
-IF _result.success
--<SysUI>.showToast("Thank you for your feedback!", "success")
--$feedback = ""
--$characterCount = 0
```

---

## 5. Image - Image Display

**Purpose**: Display images on the page.

**Use Cases**:

- Product images and galleries
- User avatars
- Logos and icons
- Background images
- Illustrations and banners

**Key Properties**:


| Property    | Type   | Description                                   |
| ----------- | ------ | --------------------------------------------- |
| `sourceUri` | STRING | Image URL (use unsplash.com for placeholders) |

**Common StyleClasses**:

- `ImageDefault` - Basic image style with border-radius
- `ImageRounded` - Slightly rounded corners
- `ImageCircle` - Circular image (for avatars)
- `ImageThumbnail` - Thumbnail with border

**VL Syntax Example**:

```vl
# Frontend Global Vars
$productImage(STRING) = "https://images.unsplash.com/photo-..."
$userAvatar(STRING) = "https://images.unsplash.com/photo-..."

# Frontend Tree
<Block-ProductCard>
-<Image-ProductImage> sourceUri:$productImage StyleClass:ImageDefault width:"300px" height:"200px"
-<Text-ProductName> value:"Premium Widget" StyleClass:HeadingSmall
-<Block-UserSection>
--<Image-UserAvatar> sourceUri:$userAvatar StyleClass:ImageCircle width:"40px" height:"40px"
--<Text-UserName> value:"John Doe" StyleClass:TextPrimary
```

---

## 6. Icon - Icon Display

**Purpose**: Display icons from multiple sources (SVG, URLs, Font Awesome).

**Use Cases**:

- Navigation icons
- Button icons
- Status indicators
- Form field icons
- Action icons (edit, delete, etc.)

**Key Properties**:


| Property  | Type   | Description                                                   |
| --------- | ------ | ------------------------------------------------------------- |
| `svgCode` | STRING | Inline SVG code (highest priority)                            |
| `url`     | STRING | Icon URL (second priority)                                    |
| `fontSet` | STRING | Font Awesome set: fa-solid-900, fa-regular-400, fa-brands-400 |
| `content` | STRING | Font Awesome Unicode code (e.g., "f007")                      |

**Priority**: svgCode > url > fontSet+content

**Common StyleClasses**:

- Basic style with font-size or width/height

**VL Syntax Example**:

```vl
# Frontend Tree
<Block-Navigation>
-<Row-NavItems>
--<Button-Home> value:"" StyleClass:ButtonIcon
---<Icon-HomeIcon> fontSet:"fa-solid-900" content:"f015" font-size:"20px"
--<Button-Search> value:"" StyleClass:ButtonIcon
---<Icon-SearchIcon> fontSet:"fa-solid-900" content:"f002" font-size:"20px"
--<Button-Settings> value:"" StyleClass:ButtonIcon
---<Icon-SettingsIcon> fontSet:"fa-solid-900" content:"f013" font-size:"20px"

# Frontend Event Handlers

<Button-Home>.@click()
-<ClientUtils>.switchRoute("home")

<Button-Search>.@click()
-<ClientUtils>.switchRoute("search")

<Button-Settings>.@click()
-<ClientUtils>.switchRoute("settings")
```

**Font Awesome Unicode Examples**:

- Home: `f015`
- Search: `f002`
- Settings: `f013`
- Edit: `f044`
- Delete: `f2ed`
- User: `f007`
- Heart: `f004`
- Star: `f005`

---

## 7. Divider - Separator Line

**Purpose**: Visually separate content sections.

**Use Cases**:

- Separate form sections
- Divide list items
- Organize card content
- Visual hierarchy
- Content separation

**Key Properties**:


| Property      | Type   | Description                                     |
| ------------- | ------ | ----------------------------------------------- |
| `orientation` | STRING | Direction: "horizontal" (default) or "vertical" |

**Common StyleClasses**:

- `DividerDefault` - Standard divider
- `DividerLight` - Light/subtle divider
- `DividerStrong` - Strong/prominent divider
- `DividerDashed` - Dashed line style
- `DividerVertical` - Vertical divider

**VL Syntax Example**:

```vl
# Frontend Tree
<Block-ProfileCard>
-<Block-Header>
--<Text-Name> value:"John Doe" StyleClass:HeadingMedium
--<Text-Email> value:"john@example.com" StyleClass:TextSecondary
-<Divider-Header> StyleClass:DividerDefault
-<Block-Stats>
--<Block-StatItem>
---<Text-StatLabel> value:"Posts" StyleClass:Label
---<Text-StatValue> value:"123" StyleClass:TextLarge
--<Divider-StatSeparator> orientation:"vertical" height:"40px"
--<Block-StatItem>
---<Text-StatLabel> value:"Followers" StyleClass:Label
---<Text-StatValue> value:"456" StyleClass:TextLarge
-<Divider-Footer> StyleClass:DividerDefault
-<Block-Actions>
--<Button-Follow> value:"Follow" StyleClass:ButtonPrimary
--<Button-Message> value:"Message" StyleClass:ButtonSecondary
```

---

## 8. Video - Video Player

**Purpose**: Display and play video content.

**Use Cases**:

- Tutorial videos
- Product demonstrations
- User-generated content
- Video galleries
- Background videos

**Key Properties**:


| Property    | Type   | Description                                   |
| ----------- | ------ | --------------------------------------------- |
| `sourceUri` | STRING | Video URL (use legitimate external resources) |

**VL Syntax Example**:

```vl
# Frontend Global Vars
$videoUrl(STRING) = "https://example.com/video.mp4"
$videoTitle(STRING) = "Product Tutorial"

# Frontend Tree
<Block-VideoPlayer>
-<Text-Title> value:$videoTitle StyleClass:HeadingMedium
-<Video-Player> sourceUri:$videoUrl width:"100%" height:"500px"
-<Text-Description> value:"Learn how to use our product in this comprehensive tutorial" StyleClass:TextSecondary
```

---

## 9. Chart - Data Visualization

**Purpose**: Display data visualizations using ECharts.

**Use Cases**:

- Sales analytics and trends
- User statistics
- Performance metrics
- Data dashboards
- Financial reports
- Business intelligence

**Key Properties**:


| Property | Type | Description                                            |
| -------- | ---- | ------------------------------------------------------ |
| `option` | {}   | ECharts configuration object (standard ECharts format) |

**VL Syntax Example**:

```vl
# Frontend Global Vars
$chartData({}) = {
  title: {text: "Monthly Sales", left: "center"},
  tooltip: {trigger: "axis"},
  xAxis: {type: "category", data: ["Jan", "Feb", "Mar", "Apr", "May", "Jun"]},
  yAxis: {type: "value"},
  series: [{
    name: "Sales",
    type: "line",
    data: [120, 200, 150, 300, 250, 400],
    itemStyle: {color: "#0052D9"}
  }]
}

# Frontend Tree
<Block-DashboardSection>
-<Text-Title> value:"Sales Dashboard" StyleClass:HeadingLarge
-<Block-ChartContainer> padding
