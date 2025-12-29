# Create Application

## Application Creation

Creating an application is the starting point of VisualLogic development. This section will detail several methods for creating new applications, including the creation of standalone applications and group applications. It will also explain the initial stage settings and management after creating applications.

### Creating a Standalone Application

In VisualLogic, the default application created is a standalone application. Standalone applications are displayed separately on the workspace and can be independently previewed, published, and deployed. They have independent frontend, backend functionalities, and databases. Here are the specific steps to create a standalone application:

1. Open the editor [VisualLogic Editor](https://editor.ivx.cn/), and select version 5.0 (or version 4.1).
2. Enter the application name in the input box below, for example, "My First VisualLogic Application."
3. Click on the theme selection box on the right; you can switch the application theme. Multiple preset themes are available for free use, defaulting to a blue PC theme. After selecting a theme, you can also manually modify styles in the application, supporting the creation of custom themes.
4. Finally, click the [Create Application] button to complete the application creation. The application will automatically open in the editor once created.

**Initial Stage Settings:**

Regardless of the type of application created, the default stage size is 375x667, which is the size of a mobile window. You can adjust the stage to PC, Pad, or other custom sizes to create applications for the corresponding scenario using the window size toggle button in the toolbar's upper right corner.

Standalone applications can operate independently, but data sharing between applications is also supported via two methods:

- Through a service-public API: Set the standalone application's data service as a public service for other applications to call via API.
- Shared account database: Enable two standalone applications to use the same account database, such as an enterprise database, to share data.

### Creating a Group Application

Group applications facilitate the management of projects, suitable for unified management of multiple different applications. After creation, they will be displayed in a group on the workspace. Group applications can conveniently realize data interchange using in-group databases, in-group microservices, etc. The steps to create a group application are as follows:

1. Open the editor [VisualLogic Editor](https://editor.ivx.cn/), and select the second option on the left "Create Group Application."
2. Fill in the group information on the right, with the group application title as a required field, such as inputting "VisualLogic Learning Project." The group cover and description can be left blank. Click the [Next] button.
3. Create an in-group application for the group. Follow the same method as creating a standalone application, first selecting the suitable version and application type, then entering the application name, such as "First In-Group Application," and then clicking the create in-group application button.

You can think of a group as a file set, with in-group applications being the application files within the set. Multiple different types of in-group applications can be created and migrated within one group.

4. After opening the group application, click on the group application management menu at the top to create a new in-group application, and proceed with the previous steps to continue adding in-group applications.

> Tips: If a group application is mistakenly created as a standalone application, it can be migrated into the group application management; if you want to convert an in-group application to a standalone application, click on the menu bar to save it as a standalone application.

### Application Creation Complete

1. After creation, the VisualLogic editor will automatically open the application.
2. Additionally, you can view and open all created applications on the workspace. Applications can be opened in two ways:
   - **Open in Current Window**: Click the [Develop] button; the application will open in the existing window, which is the default setting.
   - **Open in New Window**: Click the new window open icon to the right of the [Develop] button; the application will open in a new tab in the browser. If the developed application content is extensive and complex, using the new window open method is recommended to reduce memory pressure on low-performance computers.

You can also choose these two open methods from the application card on the workspace via right-click. When opening multiple applications in the current window, you can quickly switch by clicking the application tab.

For computers with lower performance, it is suggested to select the open in the new window option when developing complex applications to avoid crashes due to insufficient memory.

```mermaid
graph TB
    A[Open the Editor] --> B[Select Version]
    B --> C[Enter Application Name]
    C --> D[Select Theme]
    D --> E[Click Create Application]
    E --> F[Complete Application Creation]
    F --> G[Adjust Stage Size]
    G --> H[Standalone Application Creation Complete]

    A1[Open the Editor] --> B1[Select Create Group Application]
    B1 --> C1[Enter Group Information]
    C1 --> D1[Create In-Group Application]
    D1 --> E1[Complete In-Group Application Creation]
    E1 --> F1[Continue Adding In-Group Applications]
    F1 --> G1[Group Application Creation Complete]
```

Now, you have mastered the methods for creating standalone and group applications in VisualLogic.


# Select Application Type

## Introduction

When creating applications with VisualLogic, you can choose either the Web Application type or the Native Mini Program type for development. The fundamental development model for each type is consistent, but they have different component sets and basic configuration settings. Therefore, once an application is created, its type cannot be changed, and we need to decide the type of application we need to create before development.

## Web Application Type

This type of application will be referred to as "Web App." Essentially, it is a web application that can be deployed as a pure web application (i.e., HTML5 web page) or packaged into various mini programs (currently supporting WeChat, Alipay, and DingTalk) and native applications (iOS, Android, and Windows/Mac) using the packaging service provided by the VisualLogic platform. Whether it's a mini program or a native application, the packaging service of VisualLogic platform uses the webview (browser embedding) method to embed the pages we create into other applications. Additionally, VisualLogic provides various system interface layers, allowing us to call interfaces provided by mini programs or native applications, such as location services, device interfaces, and file interfaces, within the application.

### Creation Process

1. When creating a Web Application type, we need to choose between a relatively positioned stage or an absolutely positioned stage.
   - Relatively positioned stage: The stage and page are by default in a relatively positioned environment, with elements positioned relative to their normal positions, i.e., a flow layout.
   - Absolutely positioned stage: The stage and page are by default in an absolutely positioned environment, allowing users to freely drag and place each element. If you are familiar with the iH5 platform, you can choose the absolutely positioned environment, which offers an experience completely consistent with the iH5 platform.

### Use Cases

Web applications have a wide range of use cases, such as:

- In e-commerce, Web Apps can facilitate online shopping and payment.
- In the education sector, students and teachers can engage in remote learning and interaction via Web Apps.
- In enterprise management, employees can use Web Apps for project management, team collaboration, and data analysis.

Web Apps can be accessed via a browser without installing a client, supporting cross-platform and cross-system use. Therefore, Web Apps are set as the preferred application development type for VisualLogic developers.

### Deployment Options

1. Web Application types can also be deployed as apps, webview mini programs, and Windows desktop applications.
2. The application URL after deploying the Web can also be used for self-built applications on platforms like WeChat Work, DingTalk, and Feishu, or embedded into third-party systems and DingTalk mini programs via webview.

## Native Mini Program Type

WeChat Mini Programs (native components) represent a unique type of WeChat Mini Program. The components within use native components provided by the Mini Program and extended components on this basis. This type of mini program and the first type, web App version mini program, each have advantages, and we can choose the one that suits our needs better.

### Definition

"Native" is commonly used to describe an app or technology specifically developed for a particular environment or platform, such as WeChat native mini programs designed and developed specifically for the WeChat platform. They can operate directly within WeChat without the need for users to download or install other applications. WeChat native mini programs can directly access and utilize various WeChat features, such as payments, location services, cameras, etc.

### Creation Process

1. Native WeChat Mini Programs can be directly uploaded to the mini program platform in VisualLogic and previewed by scanning a code in the editor.
2. Support for one-click deployment as a WeChat Mini Program experience version, which can then be uploaded to the WeChat platform. After testing and review, the experience version can be submitted as the official mini program version.
3. Native Mini Programs can also be compiled as web applications (H5) to be directly previewed and deployed in a browser.

By default, native mini programs use a relatively positioned layout. While creating, you cannot choose the absolutely positioned layout, but you can achieve absolute layout development by adding an absolutely positioned container within the mini program application.

## Differences Between Two Types of WeChat Mini Programs

As previously mentioned, the VisualLogic platform offers two choices for creating WeChat Mini Programs: the general Web App and the native component WeChat Mini Program. To understand the differences between these two types of mini programs, we first need to understand the principle of WeChat Mini Programs:

### Principle of WeChat Mini Programs

WeChat Mini Programs operate on an application platform mainly consisting of three components:

1. Built-in browser within WeChat
2. Embedded JavaScript parsing engine
3. Native WeChat app communication layer

The built-in browser is responsible for rendering the interface, while the JavaScript parsing engine handles logic processing. The two interact through the native app communication layer. The purpose of this design is to separate interface rendering and logic execution to ensure a clean application structure and to some extent improve performance.

Additionally, WeChat Mini Programs further provide a webview (web browser component) to embed an online web application. In this case, the UI rendering and JavaScript logic parsing are performed entirely within the webview browser, like a normal web application.

Thus, WeChat Mini Programs offer two methods for application implementation:

1. Using the default system (separating rendering and JS parsing).
2. Using webview embedding (pure web execution, combining rendering and JS parsing).

These two methods correspond to the two types of mini programs in VisualLogic, where the first corresponds to WeChat Mini Programs (native components), and the second corresponds to WeChat Mini Programs published from a general Web App.

### Comparative Analysis

**Core Advantages of Native WeChat Mini Programs**

1. Supports publishing personal mini programs. Since the webview component is only available for corporate mini programs, personal mini programs cannot use it. This means if you want to publish a mini program under a personal account, you can only use native component WeChat Mini Programs.
2. Allows embedding of some native mini program UI components like live broadcast components and ad components. These components are provided outside the webview and cannot be embedded into a web page but must be added through native mini program components.
3. Quicker initial loading. Since web App version mini programs require the web component to load a remote URL, there is an extra loading process the first time it is opened. The application package for native mini program components is uploaded directly to the mini program platform, which WeChat caches automatically, making the first-time opening 2-3 seconds faster.

**Core Advantages of Web App Version Mini Programs**

1. Supports animations and timeline animations. As the UI rendering and logic engine are separated in WeChat Mini Programs with native components, it results in poor animation control performance. Most animations will noticeably stutter and are not suitable for commercial use. In the web App version mini programs, as they essentially are web applications, they naturally support all animation features of web applications.
2. Enables dynamic updates without re-approval. Since the essence of web App mini programs is a web page embedded in a mini program, publishing a web version of the application automatically updates the mini program content without requiring an additional review process.
3. Supports canvas and 3D worlds. Although native mini program components also provide canvas interfaces, they are very limited in functionality and cannot match the variety of Canvas and 3D world objects offered by VisualLogic. Therefore, functionalities related to canvas and 3D worlds are only available in the web App version mini programs. For instance, to create a print canvas poster generation feature, the web App version mini program must be used.
4. More extensive range of extension components. Due to numerous restrictions in web development for native mini programs, many extension components need to be redeveloped, and the development difficulty is considerable. Thus, the web App has a richer set of extension components.

### Performance Comparison

Although the official WeChat Mini Program claims better performance for native mini program components, extensive testing shows no noticeable difference between the web App version and native component mini programs except for the initial opening speed. On the contrary, the Web App version supports a wider range of browser interfaces (many of which are deliberately blocked in native mini programs), offering a smoother overall experience. Users can compare the differences between the two based on their applications and provide feedback.


# Interface Overview

### 1. Stage

In VisualLogic IDE, the stage is located at the center of the editor screen and is the core area where the interface of the entire project is displayed during editing. The stage is not only a display window but also allows users to interact with the application, enhancing development convenience. The stage visually displays all components and allows quick selection of these components by clicking. The related attributes of the selected component are updated and displayed in the property panel. The main functions of the stage include:

1. **Select Object**: Selecting a component on the stage directly locates it in the object tree, while the corresponding property panel shows its detailed attributes.
2. **Adjust Element Position**: For elements in an absolute positioning environment, users can directly drag these elements on the stage to change their position; if it is relative positioning, users can adjust the margins to achieve this.

### 2. Component Panel

The component panel is located on the left side of the work interface, listing all available components in icon form, and is the most core element in VisualLogic. Components are organized and divided into different categories based on their functions, such as system components, media components, data components, communication components, etc. Depending on the user’s development environment and needs, these components can be switched between simplified or complete modes.

- **Use Components**: Different components support different media types, such as image components requiring specified image materials, and audio components requiring specified audio materials.
- **View Documentation**: Users can hover the mouse over a component to view a brief introduction of its function and usage and can further view detailed documentation.

### 3. Object Tree Panel

Located on the right side of the workspace, the object tree panel is an important window for managing objects. All components added in the current development environment are visually displayed in a tree structure through the object tree. The functions of the object tree include:

- **Hierarchy Management**: Quickly select objects, adjust hierarchy relationships, copy or delete objects, rename objects, etc.
- **Frontend and Backend Distinction**: The frontend displays user-visible content, while the backend is used for data storage and service calls.

### 4. Property Panel

The property panel shows detailed properties of the selected component. Each component has some basic properties, such as position (X, Y) and size (width, height). By adjusting these properties, users can define the appearance and functionality of components to meet development requirements.

### 5. Event Panel

The event panel is the place for visualizing logical expressions (events, actions, conditions, loops). When users select a component in the object tree, they can add interaction logic to it in the event panel. This allows users to implement complex logical functions in a no-code manner.

### 6. Navigation and Menu Bar

- **Navigation Bar**: Provides several dropdown menu options for performing new, export, save operations, and some functional buttons for managing the current account resources.
- **Menu Bar**: Used for project preview, publishing, and collaborative development.

### 7. Stage Auxiliary Configuration Bar

The stage auxiliary configuration bar is located above the object tree panel, providing alignment settings and stage size adjustment tools, allowing users to preview and position the development environment more intuitively.

### 8. Logic Components

Logic components are used to add interaction logic to objects, including events, custom functions, action groups, services, etc., located on the right side of the object tree panel.

### Application Scenarios

The VisualLogic IDE, with its powerful visual and no-code development capabilities, provides developers with an intuitive and efficient application development environment. Through the above functional modules, developers can conveniently and quickly create, manage, and optimize applications, greatly enhancing the efficiency and quality of application development.
# Stage

### Introduction to the Stage

The stage is an important part of the VisualLogic IDE, located in the white area in the middle of the editor, also referred to as the Frontend or editor window. The stage is not just an area for displaying the layout and style of the project interface but also provides interactive features with the application during the development process, making application development more efficient and convenient.

### Main Functions of the Stage

#### 1. Select Objects

The content displayed on the stage is a tree structure composed of all added components. Each component in a visible state can be found on the stage and selected by clicking. The selected operation receives feedback in the object tree and properties panel: the corresponding component in the object tree will be highlighted, and the properties panel will automatically switch to the properties settings of the selected component. This method is particularly useful for easily locating specific components when the object tree is deeply nested, avoiding the cumbersome step-by-step search in the object tree.

#### 2. Adjust Element Positions

In the stage, elements in a relative positioning environment usually have fixed positions and can only be adjusted through margins. However, in an absolute positioning environment, such as the H5 application environment or canvas, the position of elements can be adjusted using X and Y coordinates, and these elements can be directly dragged on the stage to change their positions. This interactive approach provides developers with greater flexibility and control.

### Role of the Stage in VisualLogic IDE

As a general graphical programming language, VisualLogic's core is component-oriented programming, and the stage is an important embodiment of this concept. On the stage, developers can achieve:

- **Visual Component Arrangement**: Through the graphical interface, developers can easily design the layout and style of components without writing any code.
- **Intuitive Logic Arrangement**: Through the stage and event panel, developers can connect component behavior with application logic intuitively.
- **Rapid Development Iteration**: Combined with the complete toolchain of VisualLogic IDE, the stage provides a rapid preview and feedback mechanism, greatly speeding up the development process.

### Advantages of VisualLogic

As a component-oriented graphical programming environment, VisualLogic, with its innovative programming concepts and a rich component library, realizes the possibility of "no-code" development. Especially in Frontend development, it supports generating code in Vue or React frameworks, while in the Backend, it can generate Java SpringBoot framework code. VisualLogic's high flexibility not only makes it friendly to beginners but also provides powerful tool support for experienced programmers.

### Conclusion

The stage is the core part of the VisualLogic IDE, offering powerful component management and application development capabilities. Whether for quick project previews, precise object positioning, or flexible element position adjustments, the stage undoubtedly plays an indispensable role in achieving efficient and intuitive application development. Through the stage and other features of VisualLogic, developers can swiftly transform ideas into products, achieving high-quality software applications.


# Object Tree

## Overview

In the VisualLogic editor, the object tree is a visual system used for intuitive management of various components and modules within a project. It not only displays the overall structure of the project but also effectively manages various components, especially when there are numerous components. The object tree is divided into Frontend and Backend parts in VisualLogic. The Frontend reflects all visible parts of the project, such as page components and modules; the Backend is primarily used for storing relatively abstract functions like databases and front-end and back-end communication services. Below, we will explore the specific functions and operation methods of the object tree, which are realized through VisualLogic's no-code tools, facilitating fast and efficient application creation for developers.

### Levels and Parent-Child Relationships

In the VisualLogic object tree, the relationships between components are mainly manifested as levels and parent-child relationships.

#### Level Relationships

1. **Relative Positioning Environment**: In this environment, the level relationship affects the position between two objects. Objects with lower levels are arranged lower in the object tree and usually appear more towards the front of the interface.
2. **Absolute Positioning Environment**: The position arrangement between components is not influenced by mutual squeezing. The level relationship is reflected in the z-axis position of objects, i.e., when overlapping occurs, objects with a higher level will cover those with a lower level.

#### Parent-Child Relationships

Parent-child relationships represent the affiliation between objects. For example, a container can contain multiple components, which can be displayed in an indented manner in the object tree. Some properties of the parent object, such as position and visibility, will be inherited by child objects. Common parent objects include Frontend, rows, columns, container groups, etc.

### Object Tree Operations

The object tree not only displays the project structure but also provides various editing operations:

1. **Rename**: Double-click the object name to enter rename mode.
2. **Level Adjustment**: Adjust the level order by dragging objects.
3. **Affiliation Adjustment**: Drag objects from one parent to another.
4. **Management Operations**: Supports basic operations such as copy, cut, paste, delete, and provides batch operation functions.

### Advanced Features of Object Tree

VisualLogic supports event binding and data reference operations of components through the object tree.

#### Object Binding and Reference

Once a component is event-bound, a hollow circle will appear on its right side, which you can click to view all events involving that component. Furthermore, VisualLogic supports the implementation of associations between component properties through data binding, marked by blue and yellow arrows indicating this relationship.

#### Cross-Application Operations

VisualLogic allows objects to be copied from one project to another, making it convenient for reuse in different cases.

```mermaid
graph TD;
    A[Start] --> B[Select object to copy];
    B --> C[Open target project];
    C --> D[Select paste location];
    D --> E[Complete cross-project operation];
```

### Extended Features

In addition to basic functions, the VisualLogic object tree also provides powerful right-click menu functions, including:

1. **Component Conversion**: Convert objects into other types of components.
2. **Cross-Application Paste and Replace**: Copy, paste, and replace modules across different projects.
3. **Display JSON Structure**: Display the JSON structure of components, providing deep-level debugging support for developers.

These flexible feature designs give the VisualLogic object tree great flexibility and extensibility, allowing developers to engage in unrestricted project development and component management.

The VisualLogic object tree, with its visual operations, convenient management functions, and powerful extensibility, significantly improves development efficiency, enabling users to develop and maintain complex customer requirements in a simple and easy-to-use environment. This no-code platform greatly facilitates users unfamiliar with programming while also providing powerful tools for proficient developers to conduct efficient development.

### Summary

From the above description of the VisualLogic object tree, we can see it as a powerful and flexible platform offering developers a wide range of tool choices. Whether finely managing individual components or integrating complex applications, the VisualLogic object tree system can provide support. In future development, with the aid of VisualLogic, the efficiency of no-code development will be further amplified, empowering developers to achieve more efficient and flexible application development.

# Properties Panel

### Property Overview

In the VisualLogic environment, whenever an object is added, it simultaneously appears on the stage and in the object tree. Users simply click on the selected object, and the properties pane for that object will appear on the right side of the editing area. In the properties pane, users can redefine various properties to modify the object's size, position, appearance, style, data content, and functionality, allowing for detailed refinement of the object's specifics.

#### Property Definition Methods

- **Content Bar Definition**: For quantitative properties, you can define them by filling in fixed values or binding variable values in the content bar. For example, width and height properties can be defined using numbers or percentages.
- **Toggle Control**: Boolean properties are controlled by toggles, such as object visibility and whether text wraps.
- **Slider Control**: Some continuously variable properties are controlled by sliders, such as opacity and volume.
- **Dropdown Selection**: Some properties are selected using dropdown lists, like the object's border type.

### Property Classification

Objects in VisualLogic typically have the following two types of properties:

#### 1. Basic Properties

- **Dimensions (Width, Height)**: Control the object's width and height, which can be set to fixed values or percentage values based on the parent element.
- **Visible**: Control whether the object is visible, set by a toggle.
- **Opacity**: Slider or numerical input controls the object's transparency.
- **Margin**: Define the distance of the object from adjacent objects, using fixed values or percentages.
- **Background Color**: Define the object's background color using a color picker.
- **Material Resource Address**: Set the image resource address, which can be uploaded locally or from a network image address.

#### 2. Style Properties

- **Rotation**: Object rotation is defined by the origin and rotation angle.
- **Border Style**: Includes border radius, width, type, and color.
- **Shadow Effects**: Set the shadow's offset, blur, and color.
- **Effects and Styles**: Effects such as blur, brightness, contrast, saturation, grayscale, hue inversion, and rotation.

### Property Data Binding

In VisualLogic, properties can be controlled through data binding, that is, binding object properties to variables. When variable values change, property values also change in sync. This one-way data binding improves efficiency and maintainability in the development process.

### Custom Styles to Achieve Gradient Effects

Gradient effects can be achieved with custom styles in the VisualLogic property panel:

1. Add styles to the “Column” component:

   - Style Name: background
   - Style Value: `linear-gradient(-225deg, #E3FDF5 0%, #FFE6FA 100%)`
2. Add styles to the “Button” component:

   - Style Name: background
   - Style Value: `linear-gradient(120deg, #e0c3fc 0%, #8ec5fc 100%)`

Using the above steps can achieve gradient effects on components.

### Q&A

**Q: Why are some properties unadjustable after numerical binding?**

A: Because variable values can only affect property values one way, property values cannot reverse affect variable values. Thus, when a slider is bound to a value, it becomes locked and cannot slide. To achieve two-way binding, reverse assignment operations are needed.

### Flowchart

For specific processes involving multiple steps, such as "adding components and performing data binding," the following is a flowchart expressed using Mermaid syntax:

```mermaid
graph TD;
    A[Add Component] --> B[Slider and Input Box]
    B --> C[Numerical Variable]
    C --> D[Numerical Binding]
    D --> E[Select Variable]
    E --> F[Complete Binding]
```

This tutorial provides a detailed explanation of using the VisualLogic property panel, covering the definition and modification methods for object properties and styles, enabling users to quickly and effectively utilize VisualLogic tools to develop high-quality applications. Additionally, the panel's custom style feature allows for gradient effects through simple steps, further enhancing VisualLogic's flexibility in interface design.

# Saving & Rollback

### Project Saving and Save As

When editing projects with VisualLogic, the save and save as functions provide an efficient way to manage and protect project progress and versions. VisualLogic's design ensures that users can easily perform these operations, making the programming experience more seamless.

#### Project Saving

During project editing, users can save the current editing state by clicking the "Save" button in the upper right corner of the menu bar or by selecting the "File" button in the menu bar and clicking "Save" in the dropdown menu. The shortcut key "Ctrl+S" can achieve the same saving effect. These operations are entirely equivalent, allowing users to save project progress at any time according to their preferences.

The VisualLogic system includes an auto-save mechanism, automatically saving every 5 minutes and only displaying the most recent auto-save version. However, manual saving not only generates new historical versions but also allows users to load saved historical versions through the history version rollback feature. This feature emphasizes the importance of regular manual saving, effectively preventing the loss of work progress.

#### Project Save As

If you need to try new things based on the current project without affecting the original version, you can use the save as function. By selecting "Save As" in the dropdown menu of the "File" button in the menu bar, you can complete the operation. The system will prompt the user to choose whether to load the new case or cancel the operation to continue with the current interface. The save as function is particularly suitable for testing different project versions, avoiding the overwriting of existing versions due to auto-save operations during previews.

### Historical Version Rollback

VisualLogic provides historical version management functions, allowing users to load previously saved project versions as needed. Each manual save creates a historical version, and the automatically saved versions generate an archive before being automatically overwritten. Users can browse and load the required historical versions through the "History Versions" option in the "File" button, allowing flexible project version management and ensuring convenient version switching and issue recovery.

### Advantages of Reducing Repetitive Work

In the VisualLogic system, users can significantly reduce repetitive work through historical version management and component reuse functions. Functions such as action groups can be reused in multiple application scenarios, greatly reducing development burden and improving production efficiency.

```mermaid
graph TD;
    A[Start Editing Project] --> B[Manual Save];
    A --> C[Auto Save];
    B --> D[Generate Historical Version];
    C --> D;
    D --> E[Historical Version Rollback Load];
    F[Choose Save As] --> G[Receive Original Project Reuse];
    F --> H[New Version Creation];
    G --> E;
    H --> E;
```

### Code Backup on Cloud Platform

When performing version management and backup on the cloud platform, VisualLogic provides a robust data protection mechanism. Application code is naturally backed up through AWS's S3 file storage service, with each save operation updating a version backup that can store up to 200 versions. Therefore, users need not worry about code loss. In addition, VisualLogic provides manual save as operations to quickly restore to any backup version when problems occur.

### Conclusion

VisualLogic simplifies project management complexity by providing powerful saving, save as, and historical version management functions, ensuring that users can easily maintain productivity and progress security during development. These features demonstrate VisualLogic's significant advantages in simplifying program development, enhancing efficiency, and ensuring data security, providing a straightforward and effective no-code programming experience for developers and learners.

# Collaborative & Version Control

### VisualLogic in Multi-Person Development and Version Management

#### Explanation of Multi-Person Development

When building large-scale projects, the combined efforts of team members are crucial. VisualLogic Enterprise Edition specifically provides multi-person development functionality to support this collaboration. In multi-person development mode, project developers can independently develop modules and upload their work to the main branch through the push function. The main branch serves as the remote stable version for all developers, similar to Git, ensuring a step-by-step development process and project compliance.

#### Multi-Person Development and Version Management

**Project Member Invitation**

On the stage of each case, we can see a "Multi-Person Development" button. Clicking it will bring up the invitation panel, where we can input the member accounts we need to invite and assign their roles in the project. For example, developers can develop and modify cases, while administrators can invite or remove project members on this basis. The initiator will automatically become the owner of the case and have the highest authority.

**Pull and Push of Multi-Person Development**

After enabling multi-person development mode, we can access version records and version rollback functions through the history records under "Multi-Person Development." The system automatically creates a non-local main branch (master) as the remote stable version for project development, and each member also has their local branch for independent development tasks. By selecting any branch point in this branch map, the version information on the right will be highlighted to help us load the historical version list.

- **Pull**: Obtain the latest development results of the team and continue your development tasks combined with existing content. Ensure we can always pull the latest version on the main branch and merge it with the local version to form a new local version for continued development.
- **Push**: Submit completed development tasks and push the results of the local branch to the main branch to update the main branch version. Before pushing, ensure the local version is a clone of the latest main branch version; otherwise, you need to pull and merge.

**Multi-Person Development Conflicts**

Conflicts are inevitable when multiple members modify the same object. Even if the local version has been merged into the latest main branch version during push-merge, if there are different settings for the same object, the system will trigger a conflict during pull-merge. This conflict is usually resolved by marking differences, requiring internal coordination among developers to retain necessary changes. Here are some situations that can lead to conflicts:

- **Modification Conflict**: Local and remote modifications to the same object are inconsistent.
- **Deletion-Modification Conflict**: One side modifies while the other deletes the same object.
- **Object Position Conflict**: The position of the same object differs in different pages.
- **Object Order Conflict**: The position order of objects in the object tree changes.

The most effective solution is to properly plan each member's development responsibilities and minimize operations on the same object.

#### Efficient VisualLogic Development

VisualLogic supports multi-person development, with particular emphasis on efficient data reading and writing. Frontend and Backend are processed in the same editor, supporting name searching for components/action groups to improve development efficiency. The test function is always available, and erroneous components in the network environment will prompt specific information. The release process is independent of the listing process, allowing flexible rollback to the listed version to reduce losses caused by bugs.

#### Development Resource Isolation

On the VisualLogic platform, isolation of development resources can be achieved by registering multiple organization accounts to ensure each enterprise account's resources are independent. Even when sharing database instances and file buckets, isolation is confirmed through different libraries and paths. In private deployment environments, independent database instances can be configured for each institution, and despite resource isolation, resources can still be shared on the platform through the VisualLogic App Store, component market, etc.

#### Reducing Repetitive Work

VisualLogic reduces repetitive work through extended components and small modules. Action groups can be reused, and historical version management supports version rollback at any time, helping developers avoid cumbersome setups.

#### Code Backup and Security

Applications created on the VisualLogic platform have their code stored in AWS S3, providing versioning and backup multiple protections. Each save creates a version, with up to 200 versions stored, ensuring data is not lost due to misoperation or deletion. Code files are encrypted and stored on S3, and encrypted into binary files during upload to ensure security.

### Multi-Person Development Flowchart

Below is the VisualLogic multi-person development flowchart to show more clearly the interaction process between multiple steps:

```mermaid
graph TD;
    A[Start Multi-Person Development] --> B[Choose to Create Project]
    B --> C[Invite Project Members]
    C --> D[Members Accept Invitation and Join]
    D --> E[Start Local Development]
    E --> F[Pull the Latest Main Branch Version]
    F --> G{Is there a Conflict?}
    G -->|Yes| H[Solve Conflict]
    G -->|No| I[Continue Development]
    I --> J[Complete Development Task]
    J --> K[Push to Main Branch]
    K --> L[Update Main Branch Version]
    L --> M[Complete]
    H --> G
```

Through the above content, you can understand the complete set of solutions provided by VisualLogic in multi-person development and version management. Its unique component-based programming approach offers modern teams the possibility of efficient collaboration. If you have any questions about the process or usage, please feel free to consult and optimize based on your needs.

# Configuration Panel

### Overview

Through the configuration button on the navigation bar, we can open the application's configuration panel. The configuration panel is an important part of the VisualLogic development environment, mainly including the following settings: domain configuration, SEO configuration, interface configuration, environment variables, etc.

### Application Configuration

Application configuration information is the application's "peripherals," which are two independent parts from the application logic built in the object tree, property panel, and event panel. All application configuration information is only associated with the current application. When the application is saved or sent to others, this configuration information will not be retained. The saved configuration information will only affect the preview version of the application, but upon application release, it will be automatically synchronized to all released versions. Regardless of whether the application is listed, all released versions share one configuration. Therefore, modifying the application configuration will affect the currently listed application.

### Service and Database Resource Rule Configuration

In the permissions management page of the User Center, the rule configuration for services and database resources can effectively manage application resource access permissions. The service permission configuration panel is displayed as a tree table, including resource names, descriptions, and rules. The service structure is automatically generated by the object tree of the application, providing a service group to manage all its services. Each resource can set one or more rules; if none are set, the parent-level rules are followed, and child-level rules take precedence over parent-level ones. Additionally, the database configuration panel allows administrators to set independent rules for tables or fields within the application. Multiple rules can be added through buttons, supporting JSON or future visual configuration for rule settings.

### Domain Configuration

Users can customize the domain for previewing or publishing applications through domain configuration. This can simplify domain management and accelerate application development. This feature can also meet specific needs in different scenarios, such as WeChat public account or merchant account configuration.

### SEO Configuration

SEO configuration helps VisualLogic platform's single-page applications overcome search engine-unfriendly issues. By enabling Backend pre-rendering, applications can be rendered as static HTML when a search engine crawler requests the page. SEO configuration allows setting cache times for static pages to improve response speed for crawler requests.

### Interface Configuration

Interface configuration centrally manages third-party interface components encapsulated by VisualLogic. Users can configure the application ID and secret for third-party interfaces (such as mini-programs, maps, WeChat public accounts) here. Once configured, they can be saved and reused in other applications.

### Environment Variables

Environment variables are an important configuration to change application behavior without updating the application code. In private deployment, environment variables are stored in the configuration file, making it easy to manage differences between test and production environments. Since they are separate from the application code, sensitive information in environment variables will not be leaked when the application is sent.

### Panel Component Functional Features

Panel components have pull-up, pull-down scrolling capabilities, supporting custom refresh interactions. These features are very useful for loading long pages or dynamic content. Developers can flexibly combine various elements on the panel and achieve complex and dynamic interaction logic through the event panel.

### Q&A

After the application release, all versions share one configuration, so any modifications on released versions need careful consideration to avoid impacting the currently listed application. Component event settings need to be carried out under specific objects and event triggers, making VisualLogic's programming experience friendly enough even for users without a programming background, simplifying the complexity of application development.

```mermaid
graph TD;
  A[Open Configuration Panel] --> B[Select Configuration Type];
  B --> C["Domain Configuration"];
  B --> D["SEO Configuration"];
  B --> E["Interface Configuration"];
  B --> F["Environment Variables Configuration"];
  C --> G[Complete and Save];
  D --> G;
  E --> G;
  F --> G;
  G --> H[Preview or Publish Application];
```

Through a detailed configuration panel and flexible development environment, VisualLogic significantly improves actual development speed and efficiency, enabling both programmers and non-programmers to quickly build and publish their applications.

# Action

## Actions

Actions play a crucial role in the event flow and can be understood as the intuitive response of a subject to a triggering object or event. When an event is triggered, all subsequent actions that occur and execute, such as a simple variable assignment process or the hiding and displaying of a visual component, can be categorized as actions. In the framework of actions, there are two key elements: the target object and the target action. The target object refers to the subject executing the action, while the target action is the actual content being performed.

### Adding Actions

In the event panel, there are two ways to add actions to an event:

1. **Using the button to add actions**: When clicking the add action button at the top of the event panel, the new action is added based on the state of the "sub-layer/same layer" button and the position of the current selection, usually added to the end of the action list.

2. **Adding actions below existing actions**: Clicking the "+" sign on an existing action allows inserting a new action directly below it.

When an action is created, it is initially blank. At this time, we need to specify the target object and the specific target action for this action.

- **Target object selection**: You can select it on the object tree through the triangle pointer on the interface or directly from the dropdown menu of the object list.

- **Target action selection**: It can only be selected through the dropdown menu. Some target actions require parameters; in this case, the user must use the formula editor to fill in all necessary parameters to ensure the action can run as expected. For example, to generate a random number, the user needs to set the maximum and minimum values in the formula editor.

## Action Execution

Generally, actions will be executed one by one in the order added in the event panel after the event is triggered. Each action needs to wait for the previous one to complete before starting. This sequential execution mechanism ensures that the logic between actions can be maintained.

### Execution Mode of Actions in Services

1. Actions within a service are executed synchronously, i.e., one action is executed after another, including any time-consuming operations such as API or database operations.

2. VisualLogic's services are executed in parallel, using Go language's Goroutine at the underlying level, which provides good concurrency performance.

However, we can also change the execution order of actions by setting delays. By adding delays to specific actions, subsequent actions can continue to execute without waiting for the delayed action to complete, while the delayed action will start execution after the set delay time. This gives users more flexibility to adjust the event's response mechanism.

## Action Delays

In VisualLogic, you can click the clock icon on the right side of the action block to set delays. Actions added with delays will not hold up the execution of subsequent actions, while the delayed actions will execute after the delay time ends. This function allows developers to have greater control over the event sequence, thus achieving more complex interactive effects.

## Related Use Cases

Here are some examples illustrating how actions can show their roles in practical applications:

- **Button click to change background color property**: When a user clicks a button on the interface, it can change the background color property. Here, the button click is the triggering event, and changing the background color is the target action. This allows dynamic changes to the visual elements of the interface, enhancing the user's interactive experience.

- **Showing confirmation message after form submission**: After a user submits a form, a "Submission Successful" confirmation message is displayed on the page. Form submission is the event, and displaying the confirmation message on the screen is the corresponding action. This feedback mechanism helps improve the user's confirmation of the operation result.

- **Delayed sound effect after clicking an icon**: When a user clicks an icon, there is a delay before a sound effect plays. The click is the event, and the sound effect playback is the set delayed action. This arrangement can give users a sense of anticipation, adding fun to the interaction.

These examples demonstrate how actions include target objects and target behaviors. Through appropriate design and settings, rich and intuitive user interaction experiences can be achieved.

## Execution Parameters (Input Parameters) of Actions

Most actions of components require specifying one or more parameters, i.e., "input parameters," when invoked. When selecting an action with input parameters in the event panel, an input box for parameters will automatically appear. These input parameters can be a simple list form, such as specifying filter conditions when outputting from a database, and these conditions cannot be simply specified through an input box. Different input structures are provided in the action block according to the needs of different parameters.

## Multiple Response Actions

In practical applications, one event usually corresponds to multiple response actions to achieve complex logic assembly. In VisualLogic's event panel, all actions are executed synchronously. This means that when an event triggers multiple actions, these actions will be executed sequentially within the event.
 # Introduction to the Event Panel

### Overview of the Event Panel

#### Function Overview

The event panel is the foundation of interaction in VisualLogic projects. It is the core of the event-driven mechanism in the user interface, enabling the scripting of actions required after any event is triggered. Through the event panel, we can set and standardize the subsequent actions after an event is triggered and the specific sequence in which these actions are executed.

#### Composition of the Event Panel

As a crucial part of interaction, the event panel is based on the four key elements of interaction. These elements include: Trigger Object, Trigger Event, Target Object, and Target Action.

1. **Trigger Object**: In the event panel, the trigger object is automatically bound. When an event is added to a component, that component automatically becomes the trigger object.
2. **Trigger Event**: Depending on the trigger object, trigger events may vary. For example, when “Frontend” is the trigger object, there might be a “URL Change” trigger event, which is not present in the “Button” component.
3. **Target Object**: Users can freely select the target object, which affects the subsequent target actions, as the executable actions vary significantly between different objects.
4. **Target Action**: These are specific actions executed by the target object and can reference event parameters. Event parameters provide information related to the trigger event, such as the X and Y coordinates in a click event.

In addition to the four elements, the event panel supports adding notes, multiple trigger events, looping operations, conditional judgments, etc., making logic control more flexible.

#### Event Panel with Services and Action Groups

In the writing of services and action groups, the panel design is similar to the event panel. The main difference is that its trigger object and event are fixed and cannot be changed. The operations for adding actions, loops, and conditions are consistent with the event panel.

### Basic Concept of Interaction Logic

The event panel is used to edit the interaction logic between components in VisualLogic. An interaction logic consists of two parts: trigger event and response action. For example, when a button component is clicked to hide an image component. Here, "Button component clicked" is the trigger event, and "Image component hides" is the response action.

Any application can be seen as a combination of a series of trigger events and response actions. For example, an e-commerce application responds to user searches, views, purchases, etc., while a chat application responds to user input, message sending, etc. Through the configuration between these components, you can achieve complex application logic.

VisualLogic components have a series of preset events and actions. Configuring these can achieve interaction between components. Multiple components can configure multiple groups of interaction relationships, and an event can also trigger its actions. Remember, the events and actions of different components may vary.

#### Event Panel Supports Local Variable Definition

Within the event panel, we can define local variables and use them in events, enhancing the flexibility of event orchestration. This supports operations like breaks in loops, adding array loops, and more.

### Basic Interaction Logic Configuration

The event panel supports basic event logic configuration. To achieve simple interaction logic in VisualLogic, you can follow these steps sequentially:

1. **Add Event**: Select a component in the object tree and click the logic tool to add an event.
2. **Configure Trigger Event**: Select an appropriate trigger event from the drop-down menu in the event panel, such as a click event.
3. **Select Response Component**: Select the response component through the object tree in the event panel.
4. **Configure Response Action**: Choose the action that the target object will execute, such as “Toggle Display.”

```mermaid
graph LR
    A(Select Component) --> B(Add Event)
    B --> C(Select Trigger Event)
    C --> D(Select Response Component)
    D --> E(Configure Response Action)
```

### Multiple Response Actions

An event usually involves multiple response actions that need to be executed in sequence. Actions can be inserted using the event panel toolbar or right-click menu. For asynchronous operations, front-end triggers or message queue components can be used.

### Action Execution Parameters

When invoking component actions, one or more parameters (inputs) may need to be specified. Some actions do not require inputs, such as image toggle display, but more operations like flipping pages or external links need detailed parameters set.

```mermaid
graph LR
    A(Select Action) --> B{Parameter Input}
    B --> C[Single Parameter]
    B --> D[Complex Parameters]
    C --> E[Set Complete]
    D --> E
```

### Action Return Parameters

After executing a component action, return parameters may be used for subsequent operations. For example, requesting an API will have returned data post-action, which can be used for UI display or saved to a database.

```mermaid
graph LR
    A(Execute Action) --> B{Return Parameters}
    B --> C[Save to Database]
    B --> D[Update UI]
```

Through the VisualLogic event panel and interaction logic functions, users can efficiently build complex application logic without deep coding knowledge, achieving diverse user interaction experiences. This component-based programming approach simplifies and makes development intuitive, showcasing the flexibility and power of VisualLogic in a no-code development environment.
 # Condition

### Application of Conditions in VisualLogic

#### Introduction to Conditions

In VisualLogic, conditional judgment is a core tool for event handling. By adding conditions to specific actions in the event panel, the project's interactive experience can become more flexible and complex. During the development process, it is often necessary to attach certain conditions after an event is triggered, so that the predetermined actions are executed only when specific conditions are met. For example, during user registration, a registration request is initiated only when both the entered username and password are not empty. This condition-based control logic plays a crucial role in ensuring the accuracy of program operation and user experience.

#### Classification of Conditions

Conditions in VisualLogic can be classified into two types based on their positions: additional conditions and branching conditions.

- **Additional Conditions**: These are conditions directly appended to the triggering event. Each triggering event can only have one additional condition.
  
- **Branching Conditions**: These are conditions added as a branch of the event. Multiple branching conditions can be added infinitely to an event.

#### Adding and Structuring Conditions

In the event panel, conditions can be added to actions through the following steps:

1. **Adding an Additional Condition**: Click the "+" icon after the event to add an additional condition to the event.
2. **Adding a Branching Condition**: Use the blue "Condition" button at the top of the event panel to add multiple branching conditions to the event.

Condition settings vary and include determining equality, greater than, belonging to a set, or a certain type, among others. If an event contains multiple condition judgments, they will be evaluated in the order they were added and execute the associated actions.

#### Expressing Conditional Logic

In the event panel, the addition of conditions to actions is handled using logical "AND" and "ELSE":

- **"AND" Logic**: Similar to `if` in JavaScript, indicating that the action is executed only when all conditions are met.
- **"ELSE" Logic**: Similar to `else if`, indicating that the next judgment is evaluated if the previous condition is not met.

For example, for the judgment of a numerical variable, when an event is triggered, if the variable `x > 0`, the related action is executed, and if `x = 0` or other conditions, the next condition judgment is entered.

#### Using Multiple Conditions and Nesting

VisualLogic supports setting multiple parallel conditions in a conditional block:

- **Parallel "AND" Conditions**: This type of condition requires all conditions to be met to enter the branch.
- **Parallel "OR" Conditions**: This type of condition requires any one of the conditions to be met to enter the branch.

Additionally, multiple conditions can be nested in specific situations, as shown in the following logical structure:
In parallel conditions (e.g., "AND"), there is an internal "OR" condition (any one met is sufficient). The result of the external judgment can be part of the overall condition judgment, allowing complex nested conditions.

#### Handling End Conditions

If all situations in the condition block are not met, the "ELSE" branch is entered, ensuring a complete and correct logical flow of the program.

In summary, VisualLogic, through a component-oriented programming approach, a highly flexible event panel, and logical structure, provides a simple yet powerful condition management mechanism. It lowers the barrier to no-code development and offers optimized choices for coders for rapid program verification and iteration.

# Loop

## Introduction to Loops

In actual development, we often need to repeatedly perform certain actions. Handling each action manually is not only cumbersome but also makes it difficult to predict the exact number of occurrences. To address this issue, VisualLogic provides convenient looping functionalities. The looping feature on the event panel is designed for this purpose. In VisualLogic, loops can be used to execute single or multiple actions multiple times. Common loop types include:

- **Count Loop**: Sets the number of iterations and terminates automatically after reaching the count.
- **Conditional Loop**: Executes actions within the loop only when specific conditions are met.
- **Object Loop**: **Can only be applied on the canvas**, used to traverse a group of specified objects.
- **Random Object Loop**: Similarly, **can only be applied on the canvas**, it chooses a set number of objects randomly for looping.

### Adding Loops

Adding loops is similar to defining conditions. In VisualLogic, users can find a purple "Loop" button at the top of the event panel, through which loops can be added. Place the actions to be looped inside the loop, and switch the loop type from the drop-down menu.

### Detailed Introduction of Multiple Loops in VisualLogic

#### Count Loop

When using a count loop, users can set a specific number of iterations for each loop. Within the loop body, actions can reference the current iteration count (starting from 0, with a maximum of the loop count minus 1) via a formula editor.

**Example:** If the initial value of the counter is 0, and we execute the loop 6 times, incrementing the counter by the current iteration count each time, the calculation formula would be 0+0+1+2+3+4+5, totaling 15. Therefore, the final value of the counter would be 15.

#### Conditional Loop

The conditional loop allows users to add conditions, executing actions within the loop body only when the conditions are met; otherwise, it exits the loop. When defining a conditional loop, special care should be taken to modify the value of the judgment variable each time an action is executed, to avoid the condition being repeatedly satisfied, resulting in an infinite loop.

**Example:** With a loop condition where an integer variable (initial value) is less than 5, the counter increments by 1. If the variable value is not updated, the system continues to execute the loop until the webpage crashes. To avoid this, adjust the variable value after each loop; for example, increment the variable by 1 each time until it equals 5, then exit the loop.

#### Object Loop

**Object loops can only be applied on the canvas**, used to traverse a group of objects of a specific type, performing actions on these objects in each iteration. The objects in the object loop need to be within a predefined object group, and the object type must be a subtype of that object group.

#### Random Object Loop

Similar to the object loop, **random object loops can also only be applied on the canvas**. This loop introduces a "random count" value setting. The random object loop selects objects for traversal based on the set random count from all those that meet the condition. If the "random count" value is left blank, its default is the total number of objects that meet the condition, thus making the random object loop equivalent to the object loop in this scenario.

### Summary

VisualLogic provides flexible loop mechanisms, allowing developers to simplify repetitive operations and quickly implement complex business logic in a no-code programming environment. These looping functions not only improve work efficiency but also make potentially complex logic simple and intuitive. By sensibly using different types of loops, developers can better achieve the desired application logic in VisualLogic.

```mermaid
graph TD;
    A[Add Loop] --> B[Select Loop Type]
    B --> C{Choose Operation Object}
    C -->|Count Loop| D[Set Loop Count]
    C -->|Conditional Loop| E[Set Loop Condition]
    C -->|Object or Random Object Loop| F[Select Object Group]
    D --> G[Execute Loop Actions]
    E --> G[Execute Loop Actions]
    F --> G[Execute Loop Actions]
```"
 # Callback
 # # Detailed Explanation of Callback Mechanism

In VisualLogic 4.1, callbacks are a unique feature of the event panel, allowing for perfect solutions to asynchronous operations in complex logic processing. Next, we will explore the definition of callbacks and their usage in the VisualLogic IDE in detail.

## # Introduction to Callbacks

Under normal circumstances, when an event is triggered, the system executes actions sequentially from top to bottom in the event panel. Each action is executed only after the previous action completes. However, for some actions that may take an uncertain amount of time (such as image uploading, database queries, and other time-consuming operations), subsequent actions may be blocked. This is where callbacks play an important role. By placing the data and actions that need to wait for time-consuming operations to complete in the callback, it ensures that subsequent actions are not blocked, and that the callback actions are executed correctly and promptly after the time-consuming operation ends.

For example, when uploading an image, subsequent actions, such as displaying the image or continuing the process, only need to be executed after obtaining the image's URL. This requirement can be well achieved through the callback mechanism.

## # Adding Callbacks

Generally, time-consuming actions will automatically include callbacks when they are created, and we only need to define the subsequent logic in the corresponding callback. Sometimes, time-consuming actions do not come with callbacks, in which case we can manually add callbacks through the "Callback" button on the event panel. This button will only highlight when an action that supports callbacks is selected.

In some cases, time-consuming actions can support multiple callback events, allowing us to add different callbacks based on different statuses. For example, image uploads can have multiple statuses, each with corresponding callback events to respond to different results.

## # Synchronous Execution without Callbacks

In updated versions of VisualLogic, the callback mechanism has been removed in favor of synchronous execution. In this mode, all event panels on the frontend and backend support synchronous execution, and the return results of action groups and functions can be directly referenced in the formula editor. This design enhances the convenience of the formula editor, eliminating the complexity of asynchronous operations for users.

## # Additional Application Examples

When customizing callbacks, adding custom callback parameters is a powerful extension feature. For instance, a rating module can accurately obtain user ratings by defining a "score" parameter in the callback and using this information in subsequent processes. Such applications extend VisualLogic's capabilities in complex interactions while maintaining the simplicity of the main logic.

## # Custom Callback Parameters

VisualLogic allows developers to add customization when defining callback parameters, such as adding a rating parameter to monitor actions after users click submit. This flexibility significantly enhances user control in event-driven scenarios and is an essential means to maintain system extensibility and simplicity.

## # Using Callback in Action Groups

Action group callbacks are another effective method for organizing complex logic. By triggering sub-events under specific conditions, control flow can be transferred to predefined sub-events, achieving the management of complex logic. The design of action group callbacks makes the main logic clear and easily extensible, while providing flexibility in organizing complex logic.

To more intuitively demonstrate the callback process, we can use Mermaid to generate flowcharts in complex processes to illustrate operational steps. Below is a sample flowchart generated using Mermaid syntax to help understand the scenarios where callbacks are applied:

```mermaid
graph TD;
    A[Start Time-Consuming Operation] --> B{Success?};
    B -- Yes --> C[Execute Callback];
    B -- No --> D[Handle Failure];
    C --> E[Subsequent Actions];
    D --> E;
```

Through the above description, I hope readers can better understand the powerful features and efficient design concepts of the callback mechanism in VisualLogic. Whether in development efficiency or system extensibility, callbacks provide strong support and convenience for VisualLogic.

# Timeline Pane lIntroduction

### Function Overview

The timeline panel is the editing panel for creating trajectory animations with VisualLogic. In the timeline panel, we can freely set up trajectory animations for single or multiple objects.

### Bringing Up the Timeline Attribute Panel

The timeline panel can be brought up in the following five situations:

1. Adding a sliding timeline below the canvas and selecting this sliding timeline will bring up the timeline panel.
2. Adding a timeline below the canvas and selecting this timeline will bring up the timeline panel.
3. Adding audio below the canvas and selecting this audio will bring up the timeline panel.
4. Adding video below the canvas and selecting this video will bring up the timeline panel.
5. Directly adding an object below the canvas or adding an object below the above-mentioned components within the canvas, and then adding a trajectory to this object. Selecting the object itself or its trajectory will bring up the timeline panel.

### Basic Editing Area

In the timeline panel, the basic editing area includes the following main elements:

1. **Current Time, Time Ruler, Thumb**
   
   The blue box on the left shows the current time, and the blue box on the right shows the time ruler. The yellow pentagon control within the time ruler is the thumb. The current time displays the time indicated by the thumb, and the time ruler displays the length of the original duration of the currently selected object. When no keyframe is selected, a gray icon in the shape of a thumb is displayed at the current time. Clicking anywhere on the time ruler or directly dragging the thumb will instantly modify the value of the current time.

2. **Step Back, Play/Pause, Step Forward**
   
   - Clicking the step back button on the left will finely adjust the thumb/current time backward by 0.05s.
   - Clicking the step forward button on the right will finely adjust the thumb/current time forward by 0.05s.
   - Clicking the play button in the middle will play the timeline animation from the current time.

3. **Zoom Out Ruler, Zoom In Ruler**
   
   The zoom out ruler button on the left is used to decrease the display of the ruler, and the zoom in ruler button on the right is used to increase the display range of the ruler.

### Usage Scenarios

The timeline has no meaning on its own; objects within the timeline must be further assigned trajectories, integrating the keyframe animations of multiple trajectories. The timeline is a special component; it is invisible and has no size upon creation. It integrates the trajectories of multiple objects within it, allowing simultaneous control of keyframe animations for multiple objects.

### Common Functions

1. **Basic Usage**
   
   Timeline components can be added below the canvas and the relatively positioned container.

2. **Basic Attribute Settings**
   
   - **Auto Play**: If the timeline animation needs to be played only after certain interactions are achieved, turn off the auto play switch; otherwise, turn it on.
   - **Loop Play**: If the timeline animation needs to play in a continuous loop, turn on the loop play switch; otherwise, turn it off.
   - **Original Duration**: Based on the animation needs, the original duration can be automatically set to control the length of the timeline.
   - **Actual Duration/Playback Speed**: To control the overall speed of the edited timeline animation, the actual duration can be set by modifying the playback speed or directly setting the actual duration.

### Specific Usage

Add materials to the timeline, and separately add trajectories for the materials that need to move; in the timeline panel, the corresponding trajectories can be intuitively edited to set their animations. Detailed operations of the trajectories can be found in the trajectory component section. Components like rectangles and ellipses added to the timeline can all have trajectories added. Detailed settings for the trajectories need to refer to the detailed introduction of the trajectory component.

### Trajectory Editing Diagram

```mermaid
graph TD;
    A(Select Object) --> B(Bring Up Timeline Panel)
    B --> C(Add Trajectory)
    C --> D(Edit Trajectory)
    D --> E(Play Animation)
```

### Conclusion

The timeline panel is one of the core tools of VisualLogic's powerful animation functionalities. Through this panel, precise trajectory animation editing for various objects can be achieved. By making reasonable use of the timeline component and its various functions, users can easily create complex animation effects.

# Basic Editing Area

### Basic Edit Zone Overview

In the VisualLogic IDE, the timeline panel's basic edit zone is a key component, comprising the following parts from top to bottom: Current Time, Step Backward, Play/Pause, Step Forward, Time Ruler, Marker, Zoom Out, and Zoom In. Each component plays an important role throughout the entire editing process.

### Detailed Functionality

#### 1. Current Time, Time Ruler, and Marker

Displayed on the left is the current time, and on the right is the time ruler. The yellow pentagon control within the time ruler is the marker. The current time will show the specific time of the marker, while the time ruler displays the length of the original duration of the currently selected object. When no keyframe is selected, the current time shows a gray icon in the shape of the marker.

Clicking any position on the time ruler or directly dragging the marker will instantly modify the current time value. For example, the marker is at the 2.71-second position.

#### 2. Step Backward, Play/Pause, Step Forward

These three buttons are used to control the movement of the marker or the current time:

- Clicking the left Step Backward button will slightly adjust the marker or current time backward by 0.05 seconds.
- Clicking the right Step Forward button will slightly adjust the marker or current time forward by 0.05 seconds.
- Clicking the middle Play button will start playing the timeline animation from the current time.

#### 3. Zoom Out, Zoom In

These two buttons are used to control the zoom display of the time ruler:

- Clicking the right icon zooms in the ruler.
- Clicking the left icon zooms out the ruler.

### Related Questions and Answers

#### Edit Zone Background Color

The background color of the edit zone can be set through the properties panel. Users can find the relevant options in the properties panel to set their desired background color to ensure visual comfort while editing.

#### Questions and Answers

**1. How to adjust the current time?**

By dragging the marker or clicking any position on the time ruler, the current time can be adjusted.

**2. How to play the animation on the timeline?**

Click the play/pause button to start playing the timeline animation from the current time.

**3. How to zoom the time ruler?**

Click the ruler zoom buttons to zoom in or out the time ruler, depending on the direction of the button clicked.

The above content comprehensively introduces the functionalities and operation steps of the VisualLogic IDE basic edit zone. Understanding these basic functions allows users to utilize VisualLogic more efficiently in application development.



# Trajectory Editing Zone

### Overview

In the corresponding area of the panel

The blue area in the above schematic (omitted) is the track editing area of the timeline panel, including from top to bottom: previous keyframe, add/delete keyframe, next keyframe, copy keyframe, paste keyframe, track operation area.

### Detailed Functions

#### 1. Track Operation Area

As shown in the above schematic (omitted), the area within the blue box is the track editing area.

- If the currently selected object and its child objects do not have tracks, the track editing area will be empty.
- If the currently selected object or its child objects have tracks, the track editing area will display the added tracks.

#### 2. Previous Keyframe, Add/Delete Keyframe, Next Keyframe

As shown in the above schematic (omitted), the area within the blue box is a set of buttons, respectively previous keyframe, add/delete keyframe, and next keyframe.

- If the currently selected object and its child objects do not have tracks, this set of buttons will be deactivated (grayed out).
- If the currently selected object or its child objects have tracks, this set of buttons will be activated (lit up).

As shown in the above schematic (omitted), after adding a keyframe at 0s to the track of Rectangle 1, this keyframe at 0s will be activated (lit up), and a yellow icon in the shape of a keyframe will appear at the current time.

- The add keyframe button will turn into the delete keyframe button, and if you click it, the currently selected keyframe will be deleted.
- To add the next keyframe, you need to deselect the current keyframe. This can be done by clicking forward/backward, dragging the cursor, or clicking anywhere on the time ruler to switch the keyframes to a deactivated state (grayed out), while the current time will show a gray icon in the shape of a cursor.

As shown in the following schematic (omitted), after deselecting the keyframe, you can drag the cursor outside for precise time points by directly modifying the current time value and pressing Enter or clicking anywhere to make the cursor jump to the time point entered. Then, click add keyframe to add another keyframe.

During the process of adding keyframes, users can record the parameters of the keyframes in real time.

- Two points need to be noted here: First, the selected item in the object tree should not be a track; in the current example, Rectangle 1 is selected.
- Second, modifying a keyframe requires ensuring the keyframe is selected and activated. As shown in the following schematic (omitted), both keyframes are deactivated, making any parameter changes to Rectangle 1 invalid.

When there are two or more keyframes in the track, you can quickly switch between selected keyframes by selecting a keyframe and clicking the activated previous keyframe/next keyframe.

### Copy Keyframe Paste Keyframe

As shown in the following schematic (omitted), when no keyframes are selected, both copy keyframe and paste keyframe within the blue box are deactivated.

When you need to set keyframes with the same parameters, you can select that keyframe. At this time, the copy keyframe on the left will be activated. Click it to copy the parameters of the keyframe. As shown in the following schematic (omitted), when the 7.13s keyframe is selected, the copy keyframe is activated.

After completing the above operations, drag the cursor to the time point where you want to set the same keyframe. At this time, the paste keyframe on the right will be activated. Click it to paste the previously copied keyframe parameters. As shown in the following schematic (omitted), the copied keyframe has been pasted at 5s.

For more detailed track animation operations, please refer to the corresponding tutorial videos.

### VisualLogic Track Editing Advantages

VisualLogic provides a universal graphical programming language and a complete IDE (Integrated Development Environment), with the core concept being "Component-Oriented Programming". This not only allows the entire application to achieve "no-code" development but also generates code in specified languages or frameworks, with high flexibility and ease of use. Therefore, whether for non-coders learning to program or for experienced programmers, VisualLogic's programming environment is very user-friendly.

The VisualLogic IDE includes many useful panels, such as the component panel, properties panel, object tree panel, events panel, and data flow panel, supporting complete event logic and data flow orchestration. The components are comprehensive and rich, enabling backend code embedding, library imports, etc., greatly simplifying development complexity and accelerating the development process.

```mermaid
graph TD
  A[Track Editing Area] --> B[Track Operation Area]
  A --> C[Keyframe Operations]
  C --> D[Previous Keyframe]
  C --> E[Add/Delete Keyframe]
  C --> F[Next Keyframe]
  A --> G[Copy/Paste Keyframe]
  G --> H[Copy Keyframe]
  G --> I[Paste Keyframe]
```

VisualLogic has significant advantages in terms of learning difficulty, development speed, and code quality. It is easy to maintain and supports multiple advanced languages, fitting a wide range of development needs.
 # Formula Editor

### Common Functions

In VisualLogic, every assignment window where users can input content is a "Formula Editor". For example, properties such as width and height in the properties pane require the use of the formula editor when in a numerical binding state. Similarly, the assignment window in the event actions pane and the numerical determination window in the event conditions pane also require its use.

The formula editor not only allows the direct input of fixed values but also allows dynamic values to be invoked by editing formulas. For example, users can invoke the properties of other objects by selection, or invoke system built-in parameters such as current value, loop count, etc., via a dropdown menu. The formula editor supports various operators to perform mathematical operations, string manipulations, and array processing, achieved through the combination of parameters and operators.

### Basic Usage

1. **Variable Reference**

   - The formula editor supports referencing object property values. For example, users can click the arrow button in front of the assignment window to reference variables, select the object, and then choose the required property in the dropdown list of the assignment pane.
2. **Mathematical Objects and Mathematical Functions**

   - The formula editor can invoke mathematical objects and functions to complete mathematical operations. For example, the format “Math.XX” is used to invoke mathematical functions, such as the floor function `Math.floor`.

### Formula Editor Use Cases

In event panels and properties panels, many scenarios require filling in values or expressions, such as mathematical formulas. VisualLogic’s formula editor provides a unified expression input box. Users can directly input values, reference objects, or use preset blocks.

### Mathematical Formulas

The formula editor supports inputting complex mathematical formulas. These formulas can consist of literals, reference blocks, and preset blocks. For example, a general variable can be set to a numerical variable plus one, and then divided by the number of rows in an object array. Mathematical formulas support the following symbols: `(), +, -, *, /, %` etc.

```mermaid
flowchart TD
    A[Input Literal or Reference Block] --> B[Input Mathematical Symbol]
    B --> C[Validate Input]
    C -->|Valid Value| D[Output Calculation Result]
    C -->|Invalid Value| E[Prompt Input Error]
```

### 5.0 Formula Editor Optimization

In version 5.0, the formula editor supports preset input blocks for colors, character concatenation, conditional blocks, and value mapping, and allows for formula nesting. Users can achieve nested structures and conditional judgments through the formula editor, making its application more flexible.

### Referencing Object Values and Calculated Functions

Besides object property references, the formula editor also supports referencing calculated functions of variable objects. This is a method of data transformation, for example, obtaining the number of elements in an array, or performing substring or replacement operations on string variables.

### Conclusion

VisualLogic’s formula editor is a powerful and flexible tool capable of handling complex formulas, supporting various data conversions and optimizations, providing users with a convenient programming experience. With the formula editor, users can quickly implement complex logical functions in a graphical environment, increasing development efficiency.
 # Compilation Preview

### Preview Feature Introduction

The preview feature in VisualLogic allows developers to generate a project version specifically for preview purposes. This version fully presents the actual effects resulting from current edits and facilitates debugging. This preview version comes with a 'preview version' watermark and cannot be distributed or used for service; it is primarily for developer testing. The preview feature in VisualLogic is billed according to cloud service rates, with new users receiving a 20 CNY credit upon registration for initial testing.

In VisualLogic, the preview feature offers multiple options to accommodate different environments and needs, ensuring developers can choose the method that best suits their application scenarios for case preview. These options include preview functions for both PC and mobile:

### PC Preview

#### Full PC Preview

- Users can trigger a full PC preview by clicking the "Full" button located in the middle area at the top of the editor.
- Upon clicking, the system will compile the project, and upon completion, a new tab will open in the current browser, automatically redirecting the user to this page where they can experience the complete application effect.

#### Current Page PC Preview

- Besides the full preview, VisualLogic also provides a "Current Page" preview option.
- The "Current Page" preview option appears when a specific page or a component under a page is selected in the object tree, allowing preview of only the current page content as the starting point of the whole application.
- If the selected item is the bottom-most page in the object tree, the "Current Page" option will function the same as the "Full" option, as full PC preview defaults to displaying the bottom-most page.

### Mobile QR Code Preview

#### Mobile QR Code Preview

- After clicking the "H5" button in the middle area at the top of the editor, the system will automatically compile, and upon completion, a QR code will be generated within the current editor window.
- Users can use mobile devices to scan this QR code to experience the application effect on their devices.

### WeChat Mini Program Preview

- By clicking the "Mini Program" button, the system will pop up a QR code for WeChat login authorization.
- Upon successful authorization, VisualLogic will start compiling and eventually provide another QR code for previewing the application on WeChat.
- Scanning the QR code with WeChat allows for application preview in the WeChat environment, offering the most realistic test results and debugging information, making it very suitable for developing WeChat mini programs.

### Overall Flow Diagram

```mermaid
graph TD
A[Click Button] --> B{Select Preview Mode}
B -- Full Preview --> C[Compile Application]
B -- Current Page Preview --> C[Compile Application]
B -- Mobile Preview --> D[Generate QR Code]
B -- WeChat Mini Program Preview --> E[WeChat Login Authorization]
C --> F[Open New Tab in Browser]
D --> G[Device Scan QR Code]
E -- Successful Authorization --> H[Generate WeChat QR Code]
```

### Summary

In the application development process, the preview feature is an indispensable component. Through the multiple preview methods provided by VisualLogic, developers can efficiently check and adjust the display and functionality of their applications, ensuring that before final release, the application functions correctly across different operating environments. Once the preview version has been tested successfully, the application can be published and launched.
 # Database fields

### Add Fields

The first step in using a database is to set up the structure of the database columns, i.e., the "fields."

#### Add Database Component

After adding a database component, you can add a column (or field) via the button in the upper right corner.

Clicking the button will bring up a field settings popup. In the field settings popup, you must at least select the field type and fill in the field name. There is also an area to set the field's index and search criteria.

#### Field Types

The field type is the format of the field, and the value in each field can only be of one type. Below is an overview of each type and its applicable scenarios:

* **Text**: Stores string type data, such as names, titles, descriptions, etc. Note that numerical information like ID numbers or phone numbers is also recommended to be stored as text type. The maximum number of characters for a text field is 65535 (English characters).
* **Numeric**: Stores numerical type data (can have decimals). Numeric fields can use greater than, less than operations in query outputs. In VisualLogic, numeric fields correspond to the double type in MySQL, with a maximum of 15 integer digits and a maximum of 5 decimal places.
* **Image**: Image fields are essentially text fields that store the URL of image resources. This field provides the functionality to view and upload images, with a maximum URL character length of 255.
* **Time**: Time fields correspond to the datetime type in MySQL and can store specific time points with millisecond precision. Time fields have time comparison functionality and can be queried in outputs.
* **JSON**: JSON fields are used to store structured data; for example, you can directly store the value of an array or a general variable in a JSON field.

#### Field Name

The field name is the name of the column and can be named according to the information the field needs to store, such as "Name." Field names in each table must be unique and cannot consist purely of numbers.

#### Indexes and Search Criteria

In VisualLogic, indexes and search criteria are used to optimize database queries. The following conditions can be set:

* **No duplicate data insertion**: Same as the Unique index in MySQL, preventing duplicate values from being inserted and speeding up queries.
* **Index**: Adds a general index to speed up queries, suitable for fields frequently used in conditional queries.
* **Case-sensitive search**: Default is case-insensitive. Select this to enable case-sensitive searches.
* **Enable full-text search**: Automates word segmentation and matching, using the full-text search library, but with higher resource consumption.

System default fields include data ID, submission user, creation time, and update time; these are auto-recorded, read-only fields.

#### Modifying Fields

Fields can be modified at any time after being set: right-click the field name to bring up the field settings panel. You can also delete the current column, change the field position, or add columns before and after the current position.

Note: If the application has been published and is running, be cautious when modifying or especially deleting fields, as this may cause the app to malfunction.

### System Default Fields

#### Default Fields in General Databases

These are some common default fields in general databases:

* **Data ID**: Unique ID, auto-increment sequence.
* **Submission User**: Records the information of the person who submitted the data.
* **Creation Time**: The time the record was created.
* **Update Time**: The time of the last update.

#### Default Fields in Functional Databases

Functional database components, such as e-commerce, voting, user, and routing tables, come with the above default fields in general databases along with specific fields according to their functionality. For example, the shopping cart table in an e-commerce component includes a "Quantity" field.

### Modifying Fields

Fields that have been set can be modified at any time. Right-click the field name to access the field settings panel, or modify the position and order of the field columns.

### Methods of Joining Fields

Before querying using database views, it is necessary to manually associate the two data tables to be joined in the backend. There are three types of joins:

1. **Inner Join**: Returns the common data from both tables that meet the ON condition, i.e., the intersection data of the two tables.
2. **Left Join**: Returns all data from the left table, even if there is no corresponding data in the right table, it will be merged and displayed as NULL.
3. **Right Join**: Returns all data from the right table, even if there is no corresponding data in the left table, it will be merged and displayed as NULL.

#### Join Fields

Link fields through an ID field, choosing inner join, left join, or right join to achieve the desired data linkage and display when adding join fields.

### Mermaid Flowchart

```mermaid
graph TD

A[Add Database Component] --> B[Click Button to Add Field]
B --> C[Field Settings Popup]
C --> D[Select Field Type]
C --> E[Fill in Field Name]
C --> F[Set Indexes and Search Criteria]
G[System Default Fields] --> H[Data ID]
G --> I[Submission User]
G --> J[Creation Time]
G --> K[Update Time]

P[Modify Fields] --> Q[Right-click Field Name]
Q --> R[Modify Field Settings]
Q --> S[Delete Current Column]
Q --> T[Modify Field Column Position]
Q --> U[Add Column]

V[Join Fields] --> W[Select Join Type]
W --> X[Inner Join]
W --> Y[Left Join]
W --> Z[Right Join]
```

### Output Data from Database View

The newly acquired joined data table is a read-only table, which means it can only be read and not written to. You can read the table contents via services or frontend calls.
 # Database Management

### Function Overview

By default, the data in the database can be freely "added, deleted, modified, and queried" through services. However, to ensure data security, it is sometimes necessary to add certain restrictions to database modification and deletion operations. We do not want our data to be easily modified by others, thereby ensuring data accuracy and database security.

While adding a layer of validation during service development can enhance security, this method increases workload and subsequent maintenance costs when there are too many services. VisualLogic presets some permission management in the database to restrict users' submission, updating, and deletion operations. By simply setting permissions on the database panel, VisualLogic will automatically perform permission verification, ensuring the validity of operations and data security. Only operations that meet the permission settings will be accepted by the database.

### Security Protection

Enabling the "Security Protection" settings can restrict users' permissions to modify and delete the database to ensure data security:

- **Update Permission Settings**:

  - Only update own submitted records: Allows users to update records they submitted by verifying identity through the read-only field "submitted by" automatically added to the database. Users cannot update data submitted by others.
  - All records cannot be updated: The current data does not allow any update operations through services.
  - Records can be updated freely: All records can be modified by anyone through services.
- **Delete Permission Settings**:

  - All records cannot be deleted: Prohibits any database data deletion operations through services.
  - Only delete own submitted records: Only the original submitter can perform deletion operations.

### Limiting Record Submissions

Enabling the "Limiting Record Submissions" settings can restrict users' permissions to submit new records to the database by setting limits on the time or number of submissions. The start and end times can limit the time period when the submission service is effective. Other settings determine whether subsequent submission services are effective based on the number of records already submitted by the user and the total number of records received by the database.

Limiting record submissions is very useful in voting scenarios. For example, by setting up a database that records voting transactions to count the number of votes cast by voters each day and determine if they still have voting rights. If it is desired that each user has only 5 votes per day, then set "daily submissions per person" to 5 times. After a user submits 5 data records in a day, they will not be able to submit new records.

#### Flowchart

```mermaid
graph TD
    A[Set Database Permissions] --> B[Enable Security Protection]
    B --> C[Set Update Permissions]
    B --> D[Set Delete Permissions]
    C --> E[Only update own submitted records]
    C --> F[All records cannot be updated]
    C --> G[Records can be updated freely]
    D --> H[All records cannot be deleted]
    D --> I[Only delete own submitted records]
    
    A --> J[Limit Record Submissions]
    J --> K[Set Time Period Limit]
    J --> L[Set Submission Count Limit]
    K --> M[Start and End Times]
    L --> N[Number of Records Submitted by User]
    L --> O[Total Records Received by Database]
```

**This part plays a crucial role in ensuring the reliability and security of VisualLogic databases. By reasonably configuring permissions, effective protection of data is achieved.**

## Security Protection

### User Permission Settings

In security protection settings, the user's update permissions can be detailed as follows:

- **Only update own submitted records**: Users can update data they submitted by verifying identity through the read-only field automatically added to the database.
- **All records cannot be updated**: The current data does not allow any updates to ensure closure.
- **Records can be updated freely**: Allows everyone to modify all data.

### Delete Permission Settings

Delete permissions are more stringent compared to update permissions. Generally, we prefer users not to arbitrarily delete data, so the restriction options include:

- **All records cannot be deleted**: Completely prohibits deletion through services.
- **Only delete own submitted records**: Only the original submitter has delete permissions.

## Limiting Record Submissions

This permission management part is used to limit users from submitting new records to the database by controlling the number of new records through time or count settings. This functionality can be achieved through the following settings:

- **Time Period Control**: Set the effective time range for submissions.
- **Count Limit**: Determines submission validity based on the number of records already submitted by the user and the total number of records received. For example, in a voting scenario, setting the daily maximum submission count to 5 controls users to vote up to 5 times a day.

### Detailed Explanation

To set limiting record submissions, in VisualLogic turn on the "Limit Record Submissions" setting. Control is achieved through the following points:

1. **Time Period Settings**: Set the start and end times when the submission service is effective.
2. **Submission Count Settings**: Based on each person's daily submission count and the total number of submissions received by the database.
3. **User Record Judgment**: Limit further submissions based on the number of records each user has already submitted.

#### Flowchart

```mermaid
graph TD
    A[Set Limit Record Submissions] --> B[Enable Limit Switch]
    B --> C[Set Time Period]
    B --> D[Set Submission Count]
    C --> E[Set Start Time]
    C --> F[Set End Time]
    D --> G[Set Daily Submission Count per Person]
    D --> H[Set Total Database Submission Count]
    E --> I[Effective Record Rules]
    F --> J[Cancel Submissions When Time Expires]
    G --> K[Daily Submission Limit of 5 Times]
    H --> L[Total Submission Control]

    I --> M[Record Effective]
    J --> N[Cancel Excess Records]
    K --> O[Daily Submission Quota]
    L --> P[Total Database Submission Limit]
```

This content is particularly suitable for scenarios that limit frequent record submissions, such as online voting, lottery activities, etc., to prevent abuse of database resources.

## Summary

Through the database management functions of VisualLogic, we can achieve fine-grained control over database permissions, including security protection, user permission settings, record update and deletion restrictions, and limit on record submissions. These settings effectively guarantee database security, control data accuracy, and avoid confusion and security risks caused by arbitrary data operations.

VisualLogic's powerful component-based design and graphical operation interface allow users to comprehensively manage and protect the database through simple configurations without writing complex code. This is more beneficial for developers to quickly deploy secure and efficient database application systems.
 # Data Prefabrication and Import

### Database Data Addition

In VisualLogic, most of the data in the database is submitted by users during application runtime (i.e., during preview or after publication). However, we can also pre-fill some test data or import some fixed data in VisualLogic's editor. For example, we can import winning numbers for a lottery or a list of products.

In VisualLogic, there are two methods to add pre-set data to the database, including manual entry and excel import. Both methods are very suitable for data management and entry according to project needs, ensuring data accuracy and convenience.

### Manual Entry

#### After Adding Fields

1. Click the “+” sign at the bottom-left corner of the database table to manually enter data.
2. After entry is complete, click the “Save Changes” button to save the entered data to the database.
   - You can also double-click a cell to manually modify existing data.

This method is very suitable for quickly supplementing and correcting a small amount of data, facilitating real-time updates and adjustments.

### Excel Import

When there is a large amount of data to be pre-set, we can choose to import from excel.

#### Import Process

1. The system will automatically recognize the first row of the excel file as field names.
2. At this time, there are two scenarios:
   - When a field in the first row of excel already exists in the database, the system will add the data in the corresponding column to the corresponding field.
   - When a field in the first row of the excel does not exist in the database, the system will automatically create a new text type field.
3. Imported excel data will by default be “added” after the existing data, rather than overwriting the data.
4. If you want the imported excel to overwrite the current data, you can add a “Data ID” column. In this case, the system will replace the records in the current database with the same Data ID as in the excel with the data from the excel.

#### Example

Suppose the existing database structure contains two fields: name and age. The excel prepared for import is as follows:

| Name      | Age  | Gender | Grade        |
| --------- | ---- | ------ | ------------ |
| Xiao Ming | 15   | Male   | Junior Three |
| Xiao Hong | 14   | Female | Junior Two   |
| Xiao Hua  | 16   | Male   | Senior One   |

After importing the above excel, the database will automatically create the “Gender” and “Grade” fields and save the data below the existing data.

#### Operation Flowchart

```mermaid
graph TD
    A[Prepare Excel File] --> B[Check First Row Field Names]
    B --> C{Field Exists}
    C -->|Yes| D[Add Data to Corresponding Field]
    C -->|No| E[Create New Field and Add Data]
    D --> F[Save Data to Database]
    E --> F[Save Data to Database]
```

### Advantages of VisualLogic

1. **Convenient Data Management**: VisualLogic supports both manual entry and excel import, making it convenient for users to manage data as needed.
2. **Efficient Data Entry**: Supports quick entry of large amounts of data, improving work efficiency.
3. **Flexible Data Update**: Real-time correction of data can be achieved through manual methods, maintaining data accuracy and timeliness.
4. **Automated Field Creation**: Automatic field recognition and creation during excel import simplify the configuration process.

The component design philosophy of VisualLogic, “Component-Oriented Programming,” allows users to easily pre-fill and import database data, and implement comprehensive data processing and logic control using various components. By making effective use of the functions of the VisualLogic platform, application development efficiency and data management convenience can be greatly enhanced.
 # The process of publishing and launching a web application

### Overview

In this document, we provide a detailed description of the process to publish and deploy a Web-based application. This includes the operations on the publishing interface, the backend processes during publishing, and various publishing considerations. Through this document, we hope you can deeply understand the underlying principles of publishing applications as Web in VisualLogic, thereby better maintaining online projects and saving unnecessary waiting time and costs for publishing reviews.

### Publishing Interface Operation Process

Regardless of the type of application we create, there is a "Publish as Web" option in the publishing options:

After clicking publish, the publishing configuration interface will appear. Here, we temporarily use the default configuration, click Next, and after confirming the publishing, the backend publishing process will take place, and a success prompt will appear:

Click "Confirm" to directly open the play URL of the test published version:

Note, the aforementioned test publish URL has a URL parameter, `?version=1`, i.e., version number 1. Each time you publish, this version number will increment. For instance, the test publish URL will become:

Each time you access the test published version, charges are based on cloud service usage, but there is no limit on the number of times it can be played. However, it is not recommended to use it as an official sharing/distribution URL.

### Application Deployment Principles

After completing the test publish, we can proceed to deploy on the Workbench. Before introducing the workbench interface operations, let's understand the principles of deployment.

The process of deployment is essentially binding the official application URL to a certain test publish URL. Suppose our case has been test published three times, producing three test versions, respectively:

- Version 1: `https://xxxxx/J6ZMJfKY?version=1`
- Version 2: `https://xxxxx/J6ZMJfKY?version=2`
- Version 3: `https://xxxxx/J6ZMJfKY?version=3`

Now, we want to officially deploy this application so that everyone can access version 3. The deployment process is actually binding the official application URL (the one without a version parameter) to version 3:

After completing this binding process, opening the official URL will have the same effect as opening the version 3 URL, because essentially, they are version 3. The official URL, as the production URL, incurs charges based on cloud service usage.

If no binding is performed (i.e., not deployed), directly accessing the URL without the version parameter will prompt:

The reason is that the official URL at this time is not bound to any version, hence it is essentially a "null URL". Similarly, when a deployed application is "taken down", it will also cancel the binding of the official URL with any published version, making the application inaccessible.

### Application Deployment Process

Next, we introduce the application deployment process on the Workbench:

Whether it is a personal account or a corporate account, deployment needs to be performed on the Workbench:

After clicking deploy, and registering the application information for confirmation, the application can be deployed. For deployed applications, modifying the case and publishing again will not change the content of the official URL, because the official URL is still bound to the previously deployed version. Therefore, if the official version needs to be updated, the version must be updated. For instance, for deployed applications, the "Deploy" button on the Workbench will become "Update Deployed Version":

For personal accounts, clicking "Update Deployed Version" will automatically update the latest published version as the official version. For corporate accounts, an additional version management list is provided:

Any version from the historical list can be chosen for deployment. Thus, even if a certain function is accidentally broken, you can always revert to a previous normal version.

### How to Ensure the Safe Operation of Online Applications

Now that we have a basic understanding of the Web application publishing process and principles in VisualLogic, let's summarize common pitfalls during publishing based on user feedback, ensuring that projects running online do not suddenly encounter problems due to misoperation.

#### 1. Pitfall One: Modifying Database Structure During Deployment

The most common and troublesome pitfall is modifying the database structure. For deployed cases, deleting a field, changing field names, or types, etc., still share the database despite the relative independence of test published versions. Each application has only two databases: a preview version and a published version, and all test-published versions and officially deployed versions share the same database. Hence, modifying the database structure and publishing will simultaneously change this structure in the deployed version, possibly causing errors in the online version.

#### 2. Pitfall Two: Modifying Application Configuration (Especially Domain Name) During Deployment

Modifying a case's custom domain name and publishing, even without deployment, will make the previous domain name inaccessible for the case. All WeChat-related configurations and email-related configurations are shared; hence, do not modify these configurations for deployed applications.

#### 3. Pitfall Three: Upgrading Database Level During Deployment

Upgrading the database may change the database name, causing various incompatible issues. It is recommended to upgrade before officially going online. When needing to use the private database data of an online application, public service methods can be used to provide an interface to achieve this.

#### 4. Pitfall Four: Using Account/Corporate Database Modified by Other Team Members

Important databases set as account databases may be modified by team members, causing the online application to malfunction. It is best to use private databases, and when data sharing is necessary, use public service methods to share data between cases instead of directly sharing a database.

#### 5. Methods to Recover from Mistakes

If the above errors occur, the VisualLogic platform provides some recovery methods, allowing the recovery of mistakenly deleted data from the published version database within a certain timeframe (1-2 weeks), possibly involving some fees. Furthermore, corporate accounts have front-end and back-end version lists, allowing immediate rollback to the previous version if errors are found when updating the deployed version, ensuring the online version remains stable.

### Deployment Flowchart

```mermaid
graph LR
A[Select Publish as Web] --> B[Publish Configuration]
B --> C[Confirm Publish]
C --> D[Success Prompt]
D --> E[Open Test Publish Version Play URL]
E --> F[Perform Deployment Operation]
F --> G[Bind Official URL to Latest Version URL]
G --> H[Deployment Successful]
```"
 # Preview release domain configuration.
 # # What is a Domain Name

Any device on the internet, including servers or our personal computers, has a corresponding address, which is the IP address (e.g., 192.168.1.1). All devices access each other through this unique IP address. Since IP addresses are not easy to remember, the domain name system was invented to map an IP address to a domain name, such as www.google.com or www.baidu.com. When we enter a domain name in the browser, the browser will query the Domain Name System (DNS) to find the current IP address corresponding to the domain name and then access the server associated with that IP address.

Note that a domain name is not the same as the "URL address" we usually refer to but is only part of the URL address. For example, in the case of our documentation center:
https://www.VisualLogic.cn/docview?lang=zh&page=r9136l173l194&type=doc
only "www.VisualLogic.cn" is the domain name, while the rest constitutes the remaining part of the URL.

In any web application released on the VisualLogic platform, the protocol is "https://", which is an encrypted HTTP access; the domain name and access path need to be specified during the release (we will explain the specification method in detail in the next section), and URL parameters are an additional part of the URL. If necessary, we can define the handling logic for different URL parameters within the application.
 # # Custom Domain vs Public Domain
## # Advantages and Disadvantages of Public Domain

Any web application needs a corresponding domain name when published, and we can set the domain name in the VisualLogic release settings. If you do not have your domain name temporarily, you can choose to use the platform’s public domain name. The current platform public domain name is xxxx.v4.h5sys.cn, where "xxxx.v4" is the subdomain name, and each user has a unique subdomain, while "h5sys.cn" is the unified public domain name of the platform.

Since all platform users share the platform's public domain, this has certain risks. Particularly when our application needs to be published in a WeChat browser or embedded in a mini-program as a webview, the WeChat platform will review the content published on it. If any user publishes violating content using the platform's public domain, it may cause the root domain (currently h5sys.cn) to be blocked. Once the root domain is blocked, all cases published using the platform's public domain cannot be opened on the WeChat platform. Although it usually gets unblocked in about half a working day after an appeal, it will still severely impact our applications being published.

To ensure the security of the platform's public domain (and to ensure the whole platform’s service compliance), we will review all applications published on the VisualLogic platform. However, due to the complexity of VisualLogic applications and the dynamic nature of the content (for example, a chat or comment-supported application with users publishing inappropriate remarks being reported), we cannot 100% guarantee that all applications published with the platform’s public domain will comply with WeChat regulations. Therefore, for applications that need to be published on the WeChat platform, we recommend using a custom domain to ensure safe access to the application.
 # # How to Apply for a Custom Domain

## # Domain Application

You can purchase a domain from various domain service providers, with prices varying depending on the rarity of the domain, typically ranging from tens to thousands of RMB per year. The registration processes are generally similar. Using Aliyun (Alibaba Cloud) as an example in China, search for and select the domain you want to purchase, then check out and pay, and the domain will belong to you.

The only consideration when applying for a domain is choosing a quality domain registration service provider:
- The price of the same domain can vary across different service providers, related to the DNS server, where stable DNS services ensure the normal and stable operation of the domain.
- Choosing a domestic or foreign domain registration service provider, with domestic registrations typically cheaper but requiring real-name verification, making the process relatively more tedious.
- Domain registration is unrelated to the server address, and purchasing a domain does not affect the server usage. Just choose based on the above points and your situation.

Here are some common domain registration service providers:

**Domestic Domain Registration Service Providers**
- Aliyun and Tencent Cloud, are major service providers in China, with easy-to-use Chinese interfaces, convenient registration, and very reasonable domain prices.

**Foreign Domain Registration Service Providers**
- Foreign domain registrations are often more expensive but do not require real-name verification:
  - Namesilo, possibly the cheapest foreign domain registration service, with cheap .com domains, consistent renewal prices, and supports Alipay.
  - GoDaddy, the world’s largest domain registrar, with relatively cheap first-year prices but potentially fluctuating renewal prices, supporting Chinese and Alipay.

Lastly, if your application is deployed domestically, the domain must be registered with the Ministry of Industry and Information Technology (MIIT), or the domain will become inaccessible after a certain period (usually less than a week). We will detail domain registration in the next section.

## # Domain Registration

As mentioned above, any domain that needs to be accessible domestically requires registration with the MIIT. Domain registration must be through a cloud service provider. Since VisualLogic platform applications use Amazon Web Services (AWS), we recommend registering your domain on AWS:

If you want to use resources in the Ningxia region, you need to complete the ICP registration in the Ningxia region by contacting Xiyun Data:
- Email: icp@nwcdcloud.cn
- Phone: 10100966-4
- Self-service registration platform: https://icp.nwcdcloud.cn
- Company URL: http://www.nwcdcloud.cn

Note that domain registration typically takes 2-3 weeks, so ensure registration is completed before the formal deployment of the application.

If the website domain already has a registration number but was not registered with Xiyun Data (e.g., Tencent Cloud, Aliyun), you can apply for "new access" via Xiyun Data's self-service platform, and once approved by the management bureau, the website can be deployed on Ningxia region servers. Registration itself does not affect the use of other cloud servers.

Lastly, VisualLogic also provides proxy servers on Aliyun and Tencent Cloud. If your application urgently needs deployment, you can temporarily use a domain registered on Aliyun or Tencent Cloud and access the application via the proxy server.

However, using a proxy server introduces an extra layer in the network access, meaning the user needs to "detour" via Aliyun/Tencent Cloud proxy servers to access the Amazon servers, which leads to:
- Relatively slower access speed
- Inability to support large-scale high concurrency applications
- Security measures not as robust as the main service cluster, exposing higher risks of DDoS attacks

Hence, we strongly recommend registering the domain directly with Amazon, and using the proxy service only as a temporary testing solution.
 # # Domain Name Resolution

## # How to Configure Domain Name Resolution

As mentioned in the first section, when a user enters a domain name, the browser requests the DNS (Domain Name System) to retrieve the IP address associated with that domain, then accesses the corresponding web application through that IP address. Therefore, after purchasing a domain, it needs to be registered in the DNS, informing the DNS of the IP address the domain points to. This is done through DNS configuration.

All domain service providers have their domain configuration interface. For example, Aliyun:

### # Step 1:
Select the domain you purchased and choose to add a record.

### # Step 2:
Add a record pointing to the VisualLogic platform's publishing domain:

- Choose the "CNAME" record type, directly pointing to a platform default domain;
- Fill in the application subdomain, like “www” or “h5”. You can set a unique subdomain for each application or share one subdomain among all applications, distinguishing them through different access paths. We recommend the latter because many third-party interface platforms, such as WeChat, require specifying a legitimate business domain that includes the subdomain and root domain. If each application uses a different subdomain, multiple configurations will be needed.
- Fill in "v4rel.h5sys.cn", which is the platform’s system publishing domain.

### # Step 3 (Optional):
Besides the publishing domain, you can also define a preview domain for the application, making it easier to use a custom domain during preview (in scenarios limiting specific domains, like WeChat official account configurations, merchant account configurations, etc., a specific domain must be used).

To configure a custom preview domain, first, configure the preview domain resolution on the domain service provider platform, similarly to Step 2, but with a different subdomain pointing to "v4pre.h5sys.cn".

Then, fill in the corresponding preview domain in the application configuration interface (preview path does not need to be configured in the DNS, just set in the VisualLogic configuration center).

### # Step 4 (Optional):
Lastly, if your application needs to be published in the WeChat browser and obtain WeChat user ID or avatar/nickname, you will need a WeChat official account and configure the domain in the official account.

### # Domain Name Resolution Configuration Flowchart

```mermaid
flowchart LR
  A[Select Purchased Domain] --> B[Add Record]
  B --> C[CNAME Record Type]
  C --> D[Fill in Subdomain]
  D --> E[Platform Default Domain v4rel.h5sys.cn]
  E --> F[Configuration Complete]
```

## Application Release and Domain Settings

### Set the Application's Publishing Domain

You can pre-configure the publishing domain and path in the application configuration interface:

Or modify the domain and path during application release:

In the domain input box, enter your custom domain, such as h5.abc.com, and in the path input box, the system provides a default path, which you can modify according to your needs, such as "123" or "doc/view/path", etc. Ensure that the same domain does not have two applications using the same path (if duplicated, a path can only access the first bound application).
 # WeChat Official Account Platform Configuration

### 1. Usage Scenarios

When publishing H5 web pages to WeChat and needing to obtain the user's nickname and avatar, corresponding settings need to be configured in the WeChat official account configuration interface. Detailed steps are as follows:

- In the configuration interface, check "Enable Authorization" in the WeChat official account section.
- Set the initial values for obtaining avatar and nickname to "Yes".

After completing the above configurations, WeChat will pop up the following authorization prompt.

Additionally, if you have your own official account, you can configure a custom official account and replace the system default official account by following these steps. In the configuration interface, fill in your own official account appID and appSecret, and set the custom authorization domain to v4rel.h5sys.cn.

### 2. Configuring Authorized Domains

After completing the basic configuration of a custom official account, it is necessary to configure the domain in the WeChat official account backend. Detailed steps are as follows:

1. **Access the WeChat Public Platform**:
   - Enter the official account and select "Settings - Official Account Settings - Function Settings".
   - Note: Only enterprise edition official accounts can be configured.

According to whether the published version and the preview version use custom domains, further configure the three domain permissions in function settings.

#### 2.1 Not Using a Custom Domain

Just fill in the platform's default universal domain.

#### 2.2 Using a Custom Domain

Configure the publishing and preview domains. The screenshot below assumes that the custom domain is ivx.cn.

After completing the business domain and js interface security domain, if an error occurs, the following operations need to be executed:

- Go to the DNS backend and temporarily point the root domain to v4rel.h5sys.cn.
- You can temporarily add a CNAME record or an A record.

After saving, you can successfully pass the WeChat file verification stage. After deleting the previous root domain record, go back to the interface configuration, fill in the appID and appSecret, and complete the custom authorization domain configuration.

### 3. Custom Authorization Domain

In the configuration center, there is a custom authorization domain feature, mainly aimed at saving the official account webpage authorization domain space. The custom authorization domain has the following advantages:

- Both publishing and previewing can use this domain without occupying two authorized callback spaces under the official account.
- Avoid the risk of pollution caused by publishing illegal content under the default authorized domain.
- The authorized domain can be different from the domain published by the application, as long as it is pointed to v4rel.h5sys.cn in the DNS backend.

### 4. Whitelist Settings

To ensure that the cases can normally call WeChat interface services, IP whitelist configuration is required. Specific operations are as follows:

- Select Basic Configuration, configure the IP whitelist.
- Add the following two addresses to the whitelist:
  - 52.83.157.21
  - 52.83.192.50

### 5. Final Publishing

After completing all configurations, republish the case, and you can successfully use the custom official account or custom domain to publish H5 cases.
 # Create a mini program

## Creating a WeChat Mini Program

To develop a WeChat Mini Program using the VisualLogic tool, follow these steps for creating and configuring the Mini Program.

### 1. Register a WeChat Platform Account

First, complete the account registration on the WeChat platform:

1. Prepare an email that has not been registered with the public platform, open platform, enterprise account, and has not been bound to a personal account as the login account.
2. Ensure that the WeChat account bound to the public platform has fewer than five accounts.
3. Follow the WeChat registration guide to complete the Mini Program registration.

### 2. Obtain and Configure appID and appSecret

After completing the Mini Program registration, obtain and configure appID and appSecret to preview and publish on the VisualLogic platform.

#### Obtain the Mini Program appID and appSecret

1. Log in to the WeChat Mini Program platform.
2. Click on the "Development" section, and record the appID and appSecret.
3. Note: The AppSecret must be saved when generated; if lost, it needs to be reset.

#### Configure appID and appSecret in VisualLogic Platform

1. Open the VisualLogic editor and enter the configuration interface.
2. In the "Mini Program/Game" configuration section, fill in the obtained appID and appSecret.
3. Save the configuration.

### 3. Publish the Mini Program in VisualLogic

1. Open the VisualLogic editor, enter the configuration center, and complete the Mini Program development content.
2. Fill in the required configuration information for the Mini Program.
3. Publish the developed content and download the generated Mini Program development files locally.

### 4. Upload the Mini Program

#### Download and Install Developer Tools

1. Download and install the WeChat Mini Program developer tools.
2. Open the developer tools and upload the unzipped project files.
3. Modify the domain and path (url) in config.js to ensure they point to the private server IP address.
4. Modify the title and description of the Mini Program in pages/index/index.js.

#### Submit the Mini Program for Review

1. Use the developer tools to upload the Mini Program to the WeChat Mini Program management console.
2. Submit for review and wait for approval.
3. Once approved, click "Publish" to officially go live with the Mini Program.

## Complete Workflow for Creating a WeChat Mini Program

```mermaid
graph TD;
    RegisterAccount-->ObtainAppIDandAppSecret;
    ObtainAppIDandAppSecret-->ConfigureAppIDandAppSecret;
    ConfigureAppIDandAppSecret-->PublishMiniProgram;
    PublishMiniProgram-->DownloadDevFiles;
    DownloadDevFiles-->InstallDevTools;
    InstallDevTools-->UploadProjectFiles;
    UploadProjectFiles-->ConfigureRelatedInfo;
    ConfigureRelatedInfo-->SubmitForReview;
    SubmitForReview-->GoLive;
```

### Submit for Review

1. Log in to the "WeChat Mini Program Developer Console".
2. Go to "Version Management" and proceed to "Submit for Review".
3. After approval, click "Publish" to officially go live.

## Review Submission Flowchart

```mermaid
graph TD;
    EnterDevConsole-->EnterVersionManagement;
    EnterVersionManagement-->SubmitForReview;
    SubmitForReview-->AwaitApproval;
    AwaitApproval-->Publish;
```"
 # Configuration of Mini Program Platform

## # Set Legal Domains

To ensure the mini-program can smoothly use backend services, we need to set the legal backend service domain on the mini-program platform. Here are the specific steps in the **Development -> Development Settings** interface:

1. Log in to the mini-program platform.
2. Find the **Legal Domain Settings** interface below the **appID** and **appSecret** sections.
3. Fill in the platform's default preview and release domains respectively:
   - `request` legal domains: `https://v4pre.h5sys.cn`, `https://v4rel.h5sys.cn`
   - `socket` legal domains: `wss://v4pre.h5sys.cn`, `wss://v4rel.h5sys.cn`
   - `uploadFile` legal domains: `https://v4pre.h5sys.cn`, `https://v4rel.h5sys.cn`
   - `downloadFile` legal domains: `https://v4pre.h5sys.cn`, `https://v4rel.h5sys.cn`

If the application is privately deployed, you need to fill in the access domain after private deployment here. If the legal domain is not correctly filled out, no backend service can be called in the mini-program.

## # Add Developers

To allow more developers to participate in the development of the mini-program and manage appropriately, you need to configure the project members of the mini-program on the platform:

1. Search for the user to be added to the project group via WeChat ID.
2. Select the corresponding user and set permissions.

In the DingTalk mini-program management backend:

1. After creating a mini-program, the creator of the current mini-program is added as a developer by default.
2. To add developers, the administrator can search and add via mobile phone number.
3. After adding developers, they can use development tools for application development.

In the Alipay mini-program management backend:

1. **Development Members**, **Experience Members**, and **Operational Members** roles are provided.
2. Enter the **Member Management** in the left directory to view and add mini-program members.
3. Search and invite members to join the mini-program development team via Alipay account.

## # Set Security Domains

While developing mini-programs, if frontend and backend services need network communication, one or more server security domains (or IPs) must be set in advance. VisualLogic implements the underlying principles of DingTalk mini-programs using Webview, which requires adding server domains to ensure all services in the mini-program work normally.

1. The HTTP security domain and Webview domain must be set the same.
2. After customizing the release domain, input the server's domain.

In Alipay mini-program development, to ensure normal use and access, H5 domain configuration is required:

1. Put the webview address into the whitelist to avoid access interception.
2. Input the released mini-program domain.
3. Follow the official H5 whitelist process to verify the domain.

## # Entity Authentication

VisualLogic mini-programs, when exported, need Webview support, hence mini-programs with personal verification cannot open content exported by VisualLogic; enterprise verification must be chosen. Enterprise verification is applicable for enterprises, individual businesses, government organizations, overseas institutions, etc.:

1. Personal Verification: Not suitable for VisualLogic mini-programs.
2. Enterprise Verification: Suitable for various institutions, each enterprise entity can verify up to 10 enterprise mini-programs and supports enabling mini-program payments.

## # Development Settings

To carry out the normal development configuration of mini-programs, you need:

1. Obtain the **Appid** and **AppSecret** of the mini-program (needed when exporting in the VisualLogic editor).
2. Fill in the server domain: prefix of the release address, such as `file82c7e019c4b2.v4.h5sys.cn`.

## # Set API Permissions

DingTalk mini-programs provide different levels of API for invocation, but the corresponding permissions must be enabled for the mini-program to meet development needs:

1. Default Basic Permissions: Enough for basic mini-program development.
2. High-Level Permissions: Apply for enabling based on mini-program needs to achieve richer functions.

## # Flowchart

```mermaid
graph TD;
  A[Log in to the mini-program platform]-->B[Enter development settings]
  B-->C[Set legal domains]
  C-->D[Fill in default or private domains]
  D-->E[Add developers]
  E-->F[Search users via WeChat or phone number]
  F-->G[Set permissions]
  G-->H[Complete configuration]
```

By configuring the mini-program platform through the above processes and detailed steps, you can ensure smooth mini-program development, fully utilizing VisualLogic's rich components and high flexibility to quickly build mini-programs meeting various requirements.
 # Set as trial version

During the Mini Program development process, setting up a trial version is an important step. It allows a specific user group to experience and test the Mini Program's functionalities early, thereby identifying and resolving potential issues. The VisualLogic tool provides an easy way to publish and manage multiple versions of a Mini Program. This article details how to set a Mini Program as a trial version using the VisualLogic tool.

## 1. Setting the Tester Whitelist

The trial version is only available to project team members and those on the tester whitelist. If you need to offer the Mini Program to users outside the development group, you must first add these users to the tester whitelist. The specific steps are as follows:

1. On the main page of the WeChat Mini Program, select "Member Management" from the left menu bar.
2. Find the "Tester Members" module at the bottom right and click the "Add" button.
3. In the pop-up input box, enter the WeChat ID of the tester to bind, and click the search icon on the right side of the input box to search.
4. After confirming that the WeChat ID found is correct, click the "Confirm" button to complete the addition.

## 2. Publishing the Mini Program in VisualLogic

In VisualLogic, you do not directly put the Mini Program on the shelf. Instead, you submit the code to the WeChat management backend, allowing subsequent operations (such as setting it as a trial version or submitting for review to go live). The steps to publish a Mini Program are as follows:

1. Open the VisualLogic tool and select the Mini Program project you want to publish.
2. Follow the prompts to submit the Mini Program code to the WeChat management backend.

## 3. Selecting the Published Mini Program Version as a Trial Version

After publishing the code, you need to set a specific version as the trial version for testers on the whitelist to test. The steps are as follows:

1. Enter the "Version Management" interface of the WeChat Mini Program.
2. Find the just-released version, click the dropdown menu on the right, and select "Set as Trial Version."
3. Confirm in the pop-up window and click "Submit."

With these steps, the Mini Program's trial version is set up, and testers can fully experience and test the newly released Mini Program. This effectively collects feedback and optimizes the product, ensuring that the quality of the officially launched Mini Program is superior.

This is the complete process of setting a Mini Program as a trial version using the VisualLogic tool. By setting the tester whitelist, publishing the Mini Program, and selecting the trial version, you can easily manage multiple versions of a Mini Program and provide customized testing and experiences for different user groups.
 # Review and publish

#### Submit for Review and Go Live on WeChat Mini Program Platform

After the previous step "Set as Experience Version," we can find the version we just published in the position where we set the experience version, and then select "Submit for Review."

#### Submit for Review

Confirming the submission for review will jump to the above page. Complete each item following the prompts, then click Submit for Review.

#### Review Version

After submission, a review version will appear. At this point, new development versions cannot be submitted for review, but you can cancel the review and resubmit. The review results will be notified through the "Notification Center" in the upper right corner (if not approved, the corresponding reasons will be given).

#### Publish and Go Live

If the review is approved, an option to publish will appear behind the review version. At this point, confirm the publishing to officially go live with the mini program.

```mermaid
graph TD;
    A[Set as Experience Version] --> B[Find Published Version]
    B --> C[Submit for Review]
    C --> D[Complete Information]
    D --> E[Click Submit]
    E --> F[Review Version Appears]
    F --> G[Notification Center Notifies Result]
    G --> H{Review Result}
    H -->|Approved| I[Confirm Publishing]
```"
 # Create a mini program.

## # 1. Register an Alipay Account

The essence of developing an Alipay Mini Program using VisualLogic is embedded webview development, with all content based on webviews. The Mini Program is merely a container. Because adding a webview in Alipay Mini Program requires additional configuration of the H5 whitelist, and this configuration is only available to enterprise users, the current stage of VisualLogic's Alipay Mini Program development is limited to serving Alipay's enterprise users.

Go to Alipay's registration center (https://memberprod.alipay.com/account/reg/enterpriseIndex.htm), select enterprise user, and complete the registration process. You will need the enterprise's business license and legal representative information during registration.

## # 2. Join the Open Platform

Register your enterprise account on the Alipay Open Platform at https://openhome.alipay.com/ and choose to join as a self-developed developer—meaning you develop the Mini Program yourself to provide services directly to users. Follow the official guidelines for further steps.

## # 3. Create a Mini Program

After entering the Open Platform, choose "Create Now" to go to the My Mini Programs page.

The layout of the My Mini Programs page varies depending on whether a Mini Program has been created:

- If you have never created a Mini Program, click "Start Creating" to enter the creation page.
- If you have created a Mini Program before, click "Create" in the top right corner to enter the creation page.

Then, on the Mini Program creation page, fill in the Mini Program name and click "Create" to quickly set up the Mini Program.

Apart from the Mini Program name, other information can be filled in during creation or supplemented later. At this point, the Alipay Mini Program has been successfully created.

Note:

- Each developer account can create up to 10 Mini Programs.
- Mini Programs that have not been submitted for review can be deleted and will not count toward the total number.

```mermaid
graph TD
    A[Register Alipay Account] --> B[Join Open Platform]
    B --> C[Create Mini Program]
    C --> D[Fill Mini Program Name]
    D --> E[Click Create]
    E --> F[Creation Complete]
```

### 4. Submit for Review

Log in to the Alipay Mini Program developer platform, enter the version management page to submit for review. Once the review is approved, click publish to go live officially.

### 5. Upload Mini Program

After completing the Mini Program development, the developer needs to publish the Mini Program in VisualLogic. First, fill in the configuration information in the configuration center of the VisualLogic editor, and then publish it as an Alipay Mini Program. After completing this publishing process, a compressed file will be downloaded locally, which contains the Mini Program's development files. This file is used for subsequent operations.
 # Platform Configuration

## Set Secure Domain Names

To ensure that Alipay mini-programs can be used and accessed normally, H5 domain configuration for the Alipay mini-program is required. Because the content of the Alipay mini-program comes from the WebView embedded within the mini-program, the address of this WebView must be placed on a whitelist to avoid access blockage. The address that needs to be filled in here is the domain name where the mini-program is published. H5 domain configuration requires domain verification, which can be referred to in the official H5 whitelist configuration process.

### Steps

1. Access the Alipay Developer Platform.
2. Select the target mini-program.
3. Navigate to the H5 domain settings.
4. Add the domain used by the WebView.
5. Perform domain verification.

```mermaid
graph TD
    A[Access Alipay Developer Platform] --> B[Select the target mini-program]
    B --> C[Navigate to H5 domain settings]
    C --> D[Add the domain used by the WebView]
    D --> E[Perform domain verification]
```

## Add Members

Alipay mini-programs offer three types of member roles for development management: Developer, Experience Member, and Operations Member. The differences between each member role can be referenced in the official mini-program member management documentation. You only need to add collaborative developers to the project.

### Steps

1. Select Member Management in the left directory.
2. View mini-program member management information.
3. Click Add, and search for an Alipay account to invite the corresponding member to join the development team.

```mermaid
graph TD
    A[Select Member Management in the left directory] --> B[View mini-program member management information]
    B --> C[Click Add]
    C --> D[Search for Alipay account]
    D --> E[Invite the corresponding member to join the development team]
```"
 # Upload Mini Program
 # # Uploading Mini Programs

After completing the development of a mini program using VisualLogic tools, the next step is to upload and publish the mini program on the corresponding platform. The following example uses the Alipay mini program to introduce specific steps for uploading a mini program.

## # Overview of Mini Program Publishing in VisualLogic

When publishing a project as an Alipay mini program in VisualLogic, a download box will pop up upon completion, allowing users to download the project files for the Alipay mini program. The subsequent publishing and going live of the mini program rely on these project files.

## # Steps to Upload Mini Programs

1. **Download and Install Developer Tools**

   First, use Alipay's official developer tools to submit the mini program to the management console. Alipay mini programs and DingTalk mini programs share the same developer tools. If you have already downloaded the developer tools for DingTalk mini programs, you can use them directly.

2. **Open and Upload Mini Program Project Files**

   Use the developer tools to open the extracted files, which include a folder named "alipayWeb." In the tool, select and link the Alipay mini program we applied for earlier.
   
   ```mermaid
   graph TD
   A[Download Alipay Developer Tools] --> B[Extract Mini Program Project Files]
   B --> C[Open Developer Tools]
   C --> D[Select alipayWeb Folder]
   D --> E[Link to Alipay Mini Program]
   E --> F[Upload Mini Program Project Files]
```

3. **Upload the Mini Program to Alipay Mini Program Management Console**

   By using the upload button in the developer tools, you can upload the mini program to the Alipay mini program management console. Once uploaded successfully, a new development version will be generated in the backend. Each mini program can have multiple development versions simultaneously. For example, the uploaded version number might appear as 0.0.6.

### Version Management

Once the upload is successful, the management console will generate a new development version of the mini program. Developers can view and manage submitted versions in the management console to facilitate subsequent review and publishing operations.

### Summary

Through the above steps, we can smoothly upload an Alipay mini program developed using VisualLogic to the Alipay management console. VisualLogic's component-oriented programming design makes the application development process simple and efficient, while providing high flexibility and ease of use. Developers can easily complete the development, debugging, and configuration of mini programs within VisualLogic, and then use Alipay's developer tools to complete the upload and subsequent publishing work.
 # Submit for review and deploy.

### Set Up Trial Version

In the management console of Alipay Mini Program, we can select any development version and set it as a trial version. The trial version of the mini program can be accessed and used by members in the member management as trial users. However, only one trial version of the Alipay Mini Program can exist at the same time.

### Publish Mini Program

The publishing process of the Alipay Mini Program can be divided into the following steps:

#### Submit for Review

1. Submitting for review is the first step in publishing a mini program. Developers can view and add the required open capabilities for the mini program in the feature list. If the open capabilities require signing a contract (e.g., payment), make sure to complete the appropriate agreements.
2. Developers need to select any development version or trial version, fill in the review information and submit for review. The review process will be completed within 2 working days.

```mermaid
flowchart TD
    A[Select Version] ---> B[Fill Review Information]
    B ---> C[Submit for Review]
    C ---> D[Wait for Review]
    D ---> E[Review Passed]
```

#### Grayscale Testing

1. After the review is passed, the administrator can click the "Grayscale Test" button to perform a grayscale release of the version.
2. During the grayscale testing phase, developers can gradually increase the grayscale ratio. It should be noted that the grayscale ratio can only be increased, not decreased, and the maximum grayscale ratio is 50%.
3. If the version shows defects during the grayscale testing phase, developers can choose to end the grayscale test and revert to the development version for redevelopment.

```mermaid
flowchart TD
    A[Review Passed] ---> B[Grayscale Test]
    B ---> C[Increase Grayscale Ratio]
    C -->|50%| D[Continue Testing]
    B -->|Defects Found| E[End Grayscale Test]
    E --> F[Revert to Development Version]
```

#### Launch Mini Program

1. After passing the grayscale test, the administrator clicks "Submit for Launch", and the mini program can be viewed in the Alipay client.
2. When a significant defect affecting user experience is encountered in the launched version, developers can roll back the online version to the previous version to reduce the impact on user experience.

```mermaid
flowchart TD
    A[Grayscale Test Passed] ---> B[Submit for Launch]
    B ---> C[View in Alipay Client]
    C ---> D[User Feedback]
    D -->|Defect Found| E[Rollback to Previous Version]
```

Through these steps, developers can smoothly publish Alipay Mini Programs and manage and maintain versions. During the publishing process, risks can be effectively controlled to ensure the stability of user experience. The VisualLogic platform, with its high flexibility and no-code development advantages, allows developers to successfully publish mini programs even without coding knowledge, greatly enhancing development and maintenance efficiency.
 # iOS Application Release

### Release Overview

iOS app release must take place on Apple devices, and the process generally includes the following steps:

1. Apply for an App ID
2. Create a Certificate Signing Request file (CSR) in the configuration panel
3. Create a certificate (CER) on the developer website
4. Create a Provisioning Profile: release profile and test profile
5. Register the app information on iTunes Connect
6. Publish, compile, and download the IPA package in the editor
7. Conduct local testing
8. Upload using the development tool Xcode
9. Submit for review and release the app on the iTunes Connect website

The following content primarily explains steps 2, 6, and 7 within VisualLogic. It is recommended to refer to the corresponding iOS app submission process and certificate creation tutorials for the other steps. It is advisable to understand the entire iOS app release process before reading the following content in detail.

### Creating Certificate Signing Request File (CSR) in VisualLogic

1. Enter the VisualLogic editor and access the app release configuration interface through the top tool menu.
2. Click the generate button to quickly create a CSR file and enter the following details:
   - Country (must be a two-letter country code, e.g., China CN)
   - Province
   - City/Region
   - Organization name
   - Organizational unit name
   - Common name
3. Confirm and generate a CSR file, then download it locally. This file is used to create development or distribution certificates on the developer website.

### Publish, Compile, and Download IPA Package

With the CER certificate and provisioning files downloaded and the app development completed, proceed with the following steps:

1. Enter the VisualLogic app release configuration interface and fill in all details except for the CSR file:
   - Bundle ID, the app ID obtained
   - Version number, which must increment and cannot decrement
   - App name
   - Provisioning Files, configuration files obtained from the official Apple platform, click on icons for test and release versions to upload
   - Certificate, certificate file obtained from the official Apple platform, click the icon to upload
   - Icon, a PNG image of size 1024x1024. After successfully uploading the correct size and format, an upload complete icon is shown, and a thumbnail can be re-uploaded by clicking. Failure to meet size requirements will result in an unsuccessful upload.
   - Loading Image, the default startup loading screen displays a black screen with no image, to customize the loading screen for each size, click corresponding size under the loading image section to upload. You can upload 1-7 images per size or batch upload 7 PNG images with the required naming format: 640x960.png, 640x1136.png, 750x1334.png, 828x1792.png, 1125x2436.png, 1242x2208.png, 1242x2688.png.
2. Save configuration information and start app release.
3. Select the version to compile: test or release version to generate different application packages.
4. After a successful publish and compile, a window will open to confirm the download of the IPA application package, which can then be downloaded locally.

```mermaid
flowchart TD
    A[Enter VisualLogic release configuration interface] --> B[Fill in release information]
    B --> C[Save configuration]
    C --> D[Select compile version]
    D --> E[Publish and compile]
    E --> F[Download IPA package]
```

### Local Testing

Import the IPA file to an Apple device using application helpers like iTools or Pgyer for testing.
 # Release of Android Application

## Release Overview

The release of native Android applications is similar to the release process of various mini-programs, mainly including the following steps:

1. Register developer credentials.
2. Fill in the configuration on the VisualLogic platform.
3. Publish, compile, and download the application package.
4. Local testing.
5. Submit for platform review and go live with the application.

Note:
- If the app does not need to go live on Android app markets/platforms and is only for local installation and testing, you can skip the platform registration process. Go to configure application information --> Interface Configuration --> Android section, and fill in a set of test configurations.
- If the app needs to go live on one or more application markets and platforms, you must apply for a developer account according to the process and operate according to the requirements of the corresponding platform.

As there are many platforms for Android release in China, the release of applications varies according to platform differences. Detailed instructions for releasing on each platform will not be provided here. Please refer to the attached links for more information.

## Application Information Application and Configuration for Android Apps

In the Chinese Android market, there are multiple platforms, such as Tencent App Store, 360 Mobile Assistant, Huawei App Store, etc. To apply for a developer account on the platform where you want to release, you need to go to the corresponding platform and check its tutorial, for example:
- Tencent Open Platform:
  - [Mobile App Access Beginner's Guide](https://wiki.open.qq.com/wiki/移动应用接入新手指引)
  - [Register Developer Account](https://wiki.open.qq.com/wiki/注册开发者账号)
- 360 Mobile Open Platform:
  - [Register Developer Account](http://dev.360.cn/wiki)

## Configure Application Information

After obtaining the developer account and appid information, we need to return to the VisualLogic editor's configuration page to fill in the app's configuration.

### Basic Application Information Fields
- **App ID**: Enter the custom-named APP ID (format like com.app.ivx, must start with com).
- **Version Code**: Enter the version code.
- **Version Name**: The version name format must be 0.0.0 and higher than the last published version code.
- **Application Name**: Enter the application name.

### Generate KEYSTORE
Generate a KEYSTORE for each Android application you want to publish. The following information is mandatory except for "Common Name":
- **Country**: Must be a two-letter country abbreviation, such as China (CN).
- **Province**
- **City/Region**
- **Organization Name**
- **Organization Unit Name**
- **Common Name**

### Icon
The default icon for Android applications is the Android robot. To customize the icon, you need to upload the app's circular and square icons here; upload two 192×192 png images for each, and after successful upload, click the thumbnail to re-upload if needed.

### Loading Image
The Loading Image is the application's splash screen when it starts. By default, there is no image displayed on the loading screen. If you want to customize the loading images for different resolutions, click on the corresponding size under the loading image to upload each image. Batch-upload png images need to be named by size as follows: 375×667.png; 563×1001.png; 750×1334.png; 1125×2001.png; 1500×2668.png. After successful upload, click the thumbnail to re-upload if needed.

## Release Android Application and Local Testing

### Release Android Application
Release the developed Android application in the VisualLogic editor. This process compiles the project into an .apk file. Once published and compiled, a download prompt will appear to download the compiled .apk file.

### Local Testing
Use an Android emulator to test on a PC, or import the app into an Android phone using tools like Wandoujia to test it. Note:
- When sending the .apk installation package to a phone, it is best not to use WeChat communication tools, as WeChat will automatically add a suffix to the .apk file, making it unusable on the phone. It is recommended to use tools like Wandoujia to import the installation package into the Android phone.

## Official APP Packaging

Choose official packaging in the settings brought out by VisualLogic, which binds the app with the released version's backend service by default. Therefore, after exporting Android, you need to manually publish the app on the VisualLogic Workbench; otherwise, the service will not be effective. If it's a privately deployed Android APP, you need to add a custom server domain name and file address in the configuration, similar to private deployment of mini-programs. After configuring custom server domain name and file address, the packaged APP will use the custom-configured private deployment backend service by default, without the need to publish the app on VisualLogic.

## Listing on Application Market

After the official version of the app has passed functionality testing, it needs to be listed on various Android application markets. This step is similar to the creation process and requires going to the corresponding application market or platform to submit for review; only after approval can it be officially published to the corresponding market or platform. Below are detailed review instructions for common platforms:
- [Tencent Developers Platform](https://wiki.open.qq.com/wiki/创建新应用)
- [360 Mobile Open Platform](http://dev.360.cn/wiki/index/id/21)

## Release Flowchart

```mermaid
graph TD
  A[Register Developer Credentials] --> B[Fill in VisualLogic Configuration]
  B --> C[Publish and Compile Application Package]
  C --> D[Download Application Package]
  D --> E[Local Testing]
  E --> F[Submit for Platform Review]
  F --> G[Go Live with Application]
```"
 # Windows/Mac Application Deployment

## # Windows Application Deployment

### # Deployment Configuration

Publishing an application on Windows will generate an executable .exe file, which is the application itself. Before proceeding with deployment, some configurations need to be set. Enter the Windows deployment interface configuration through the toolbar at the top of the editing interface to define the output file's properties. These configurations include:

- **Title**: The name of the application post-deployment
- **Width**: The default width when the application is opened
- **Height**: The default height when the application is opened
- **Icon**: A 256x256 .ico file that will serve as the application's icon

### # Deployment and Execution

Once the configuration is complete, click the deploy button to initiate compilation. Upon successful compilation, the system will automatically begin downloading the generated application package. Save the downloaded application package to the Windows system and double-click to run the .exe application file.

```mermaid
graph TD
A[Enter publishing interface configuration from editing interface] --> B[Set the output file properties]
B --> C[Click the deploy button]
C --> D[Initiate compilation]
D --> E[Download application package upon successful compilation]
E --> F[Save locally and double-click to run]
```

### Mac Application Deployment

#### Deployment Configuration

Publishing an application on Mac will generate a .dmg image file. Similar to the Windows deployment process, some configurations need to be set in advance. These configurations include:

- **Title**: The name of the application post-deployment
- **Width**: The default width when the application is opened
- **Height**: The default height when the application is opened
- **Icon**: A 512x512 .icns file that will serve as the application's icon
- **Dmg Background Image**: The background image of the generated image file

#### Deployment and Execution

After completing the configuration, click the deploy button to enter the compilation process. Upon successful compilation, the system will automatically begin downloading the generated application package. Download the application package locally and double-click to install the .dmg image file to generate an executable .app file. Run the .app file to begin using and testing.

```mermaid
graph TD
A[Enter publishing interface configuration from editing interface] --> B[Set the output file properties]
B --> C[Click the deploy button]
C --> D[Initiate compilation]
D --> E[Download application package upon successful compilation]
E --> F[Save locally and double-click to install]
F --> G[Run .app file for testing]
```"
 # Cloud Platform Application Services
 # # Overview

Applications on the VisualLogic platform are automatically deployed to VisualLogic’s public servers, with the platform providing cloud resource services and charging based on resource usage. Additionally, enterprise users can choose to export applications and deploy them on their own servers.

The IDE provided by VisualLogic only generates “the program itself” and decouples the resources needed for backend runtime (separating code and cloud IaaS). Users can opt for private deployment on their own servers. Simultaneously, VisualLogic offers convenient cloud computing services for development debugging, previewing, releasing, and official deployment. Applications hosted on the VisualLogic platform will incur charges based on actual server consumption, including computation fees, traffic fees, and third-party API fees (deducted from the account balance and paid by the VisualLogic platform to cloud service providers).

Core cloud computing products integrated by VisualLogic include: ELB load balancing, Fargate container clusters, Lambda function computing, Aurora database, ElastiCache caching services, Dynamo database, and Stream database. Using VisualLogic cloud services will save you the time and manpower needed for server maintenance and various cloud computing products.

The VisualLogic cloud service platform charges based on the actual usage in application debug, preview, release, and deployment phases (no charge for privately deployed applications). The pricing details are as follows.
 # # Cloud Platform Cloud Computing Fees

## # Cloud Computing Service Fees

The operation of backend service components for VisualLogic applications will occupy server computing resources, for which we will charge fees. The billing is based on usage as shown in the table below, settled every 5 minutes, and differentiated between application debug and preview version fees and release version fees:

| Calculation Item | Description                                          | Unit       | Price                              |
| ---------------- | ---------------------------------------------------- | ---------- | ---------------------------------- |
| Index            | Index service addition/application service           | per time   | ¥0.0015 per time                   |
| Offline Compute  | Backend service computation in non-FC mode           | ms         | ¥0.0000054 per ms                  |
| Function Compute | JS function computation in FC mode executed by Lambda | ms         | ¥0.0000054 per ms                  |
| MySQL Database   | Aurora database service                              | ms         | ¥0.0000078 per ms                  |
| ES Database      | ES database full-text search service                 | ms         | ¥0.0000078 per ms                  |
| QuickTable       | Dynamo database service                              | CU         | ¥0.00015 per write CU ¥0.00003 per read CU |
| Log              | Dynamo database consumption for writing logs         | log entry  | ¥0.00015 per log                   |
| Queue            | None                                                 | messages   | ¥50 per million messages           |

## # CDN Traffic Fees

When VisualLogic applications load resources such as images and videos uploaded to the VisualLogic platform, CDN traffic will be generated, for which we will charge fees.

CDN traffic billing is based on usage, settled per 1GB. Given the current service architecture of the CDN, we cannot directly calculate the actual traffic generated by applications from the backend. To ensure fairer billing for users, we have developed a front-end runtime traffic statistics method, returning the actual generated traffic data from the front end. Fees will be charged when the traffic reaches the 1GB threshold. For specific pricing, please refer to the cloud resource service pricing page.

## # Third-Party API Fees

Third-party API billing is based on usage; different APIs have different settlement methods and pricing, for example, SMS and AI model usage.
 # WebApp version mini program release.
 # # What is a WebApp Mini Program

When creating a VisualLogic application, developers can choose the type of application they want to create, which is mainly divided into three categories: WebApp Mini Programs, WeChat Mini Games, and WeChat Mini Programs. The WebApp Mini Program includes a category for WeChat Mini Programs, where this Mini Program refers to the WebApp WeChat Mini Program. So, what distinguishes it from the third category—WeChat Mini Programs?

The WebApp Mini Program is not written using the native Mini Program syntax; instead, it utilizes the Mini Program's WebView component to introduce an external webpage. Developers only need to complete the development of this external webpage and present it within the Mini Program, without using any native Mini Program syntax.

Essentially, it's a webpage embedded within a Mini Program. Due to WeChat's regulatory constraints, the ability to embed webpages within Mini Programs is not currently available to individual accounts and overseas accounts. Moreover, the embedded webpage requires a custom domain to be published, so a verified domain needs to be prepared in advance.

WeChat Mini Programs, as a major category, are fully developed using WeChat's native syntax and can directly call WeChat's official APIs, although it lacks performance in animations and effects. The WebApp version of Mini Programs can break free from WeChat's environmental constraints, allowing for broader API calls and smoother animations. Therefore, WebApp and native WeChat Mini Programs each have their pros and cons, with developers able to choose based on their specific needs.

The WebApp Mini Program requires configuration on the WeChat Open Platform's H5 whitelist, a feature only available to corporate certified users, limiting WebApp Mini Programs to enterprise users only.
 # # Platform Configuration

A WebApp WeChat Mini Program also takes up a Mini Program's AppID. Since it uses WebView, it requires registration as an enterprise version of a WeChat Mini Program, with AppID and AppSecret configuration similar to that of native Mini Programs.

## # Configure Business Domain

Using the WebApp version of WeChat Mini Program requires additional configuration of the Mini Program's business domain. Log in to the Mini Program backend at `mp.weixin.qq.com`, select **Settings** -> **Development Settings** -> **Business Domain**, and add the domain module configuration.

Configuring a business domain requires an administrator to verify via scan and domain verification. To configure a business domain, download the verification file provided by WeChat and place it in the root directory of the domain. If using private deployment, upload the verification file to the private deployment server and ensure access; if using a platform's server with a custom domain, follow these steps:

1. Publish an example using the custom domain and access it once.
2. After downloading the file, obtain the filename like `cevqnm1ozm.txt`, and the file content like `3c33ee1392XXXXXXXXXe28dabf10`.
3. Add two records in the domain management backend:
   - Point the subdomain, such as `h5`, with a CNAME to `v4rel.h5sys.cn`.
   - The downloaded txt filename (without the file extension). subdomain, the record type is TXT, and the record value is the string in the txt file.

After adding, wait a few minutes and access the subdomain + filename in a browser. Seeing the txt string value indicates successful upload of the verification file. You can then return to the WeChat Mini Program and click save.
 # # Publish Mini Program

After completing a series of platform configurations, the WebApp WeChat Mini Program can be normally accessed within WeChat. Next, go back to the VisualLogic editor to publish the WeChat Mini Program.

Choose to publish as a WeChat Mini Program and use a custom domain to publish the Mini Program, ensuring the domain matches the verified business domain.

In the WebApp Mini Program publishing interface, you need to adjust the "Deployment Settings" based on different situations:

- If you want to preview the current WebApp Mini Program, choose "Preview Only." In this case, the application will only run in the WebApp Mini Program environment without restrictions on business domains, and no code will be uploaded to the WeChat public platform.
- If ready to debug and publish the WebApp Mini Program as a public version, choose "Deploy." This option will not provide a preview QR code after publishing. This will submit the WebApp Mini Program code to the WeChat public platform and create a development version, restricting the WebApp Mini Program to operate backend services within the business domain.

Subsequently, the compilation and preview of the Mini Program will begin. The Mini Program published within VisualLogic will upload to the WeChat Mini Program backend, creating a new development version. Subsequent steps to set it as an experience version or submit it for review and launch are the same as those for native Mini Programs. You can refer to the native Mini Programs' experience version documentation and review publishing documentation.

## # WebApp Mini Program Publishing Flowchart

```mermaid
graph TD
    A[Create VisualLogic Application] --> B[Choose WebApp Mini Program]
    B --> C{Need to Configure Business Domain}
    C -->|Yes| D[Log in to Mini Program Backend to Configure Business Domain]
    D --> E[Admin Scan for Verification]
    E --> F[Download Verification File]
    F --> G[Add Records According to Domain Management Backend]
    G --> H[Save Configuration]
    C -->|No| I[Directly Enter VisualLogic Editor]
    H --> I
    I --> J[Choose Publish as WeChat Mini Program]
    J --> K{Choose Deployment Method}
    K -->|Preview Only| L[Run Application but Don’t Upload Code]
    K -->|Deploy| M[Upload Code to Create Development Version]
    M --> N[Compile and Start Preview]
    N --> O[Set as Experience Version or Review and Launch]
```"
 # High Concurrency Application

**Concurrency Limitations on Public Platforms**

To ensure the security of platform application access, block malicious attacks, and keep your backend service costs under control, VisualLogic's public platform has the following application concurrency limitations:

1. Limit on the number of new application openings per minute: 1000 times (if exceeded, new users opening the application will directly return to a busy service page)
2. Maximum number of service requests processed per second: 100 (if exceeded, additional services will directly return busy information)

If your application requires higher concurrency limits, you can contact customer service for project registration. We will invite consulting experts to check your application logic to ensure that various backend operations are optimized (see Section 3, Optimization of High Concurrency Applications) and then increase the current case's concurrency limit according to your actual needs. Increasing the concurrency limit will involve certain costs, primarily including:

1. Expert case review fee: We need to examine your case logic in detail to ensure that it is optimized in terms of backend service usage, reducing the pressure on backend servers and lowering case service cost consumption.
2. Server warming-up fee: For extremely large events, such as large live events, we need to pre-launch resources for servers and databases and assign dedicated personnel to guard the site to ensure the fastest response at the event site. Depending on the specific event needs, we will charge different fees (not all events require resource warming-up; please consult customer service for details).

---

**High Concurrency Limitation Nodes**

Many applications run smoothly when the concurrency is low, but once high concurrency situations occur, various backend issues may arise. In this section, we specifically introduce the bottleneck nodes of high concurrency applications to better solve high concurrency problems.

First, the backend services of VisualLogic mainly consist of three parts:

1. **Case Opening**: Responsible for processing sample opening requests, querying the corresponding homepage JS package based on the domain name, and returning the initial index html file.
2. **Service Logic**: Responsible for processing all service logic, including manually defined backend services and services automatically generated by directly accessing the database from the frontend.
3. **Database**: MySQL database using Amazon's Aurora scalable database service.

Any service cluster has an automatic elastic scaling function. When the average load of the current server cluster exceeds a certain number, it automatically adds server resources. However, the elastic scaling of any cluster takes time (especially database clusters, which need 10-20 minutes to expand new instances). Therefore, if the request volume of any node surges, it may cause the current resources to be over-consumed and respond slowly. Thus, we have imposed default resource occupancy limits on all applications to ensure the platform runs stably overall.

For high concurrency applications, we conduct professional reviews at the case logic level to ensure that under the premise of achieving the desired effect, the computational consumption of the service logic cluster and the database cluster is minimized. Moreover, for large high concurrency live events, we also pre-enable more database resources to avoid slow service request responses during server elastic scaling.

---

**Optimization of High Concurrency Applications**

In this section, we analyze how to optimize backend service design from the perspective of case production so that the case can save backend server resources as much as possible in high concurrency scenarios.

**1. Use Push-Based Services Instead of Pull-Based Services**

Push-based services refer to the backend actively pushing information to the client, whereas pull-based services mean the client actively requests information from the backend. For example, to display user comments in real-time on the frontend, if using a pull-based service, a trigger is needed to periodically request the backend for the latest user comments and then display them on the frontend. If using a push-based service, the backend polls the database for new comments at regular intervals and pushes them to the frontend through a connection if there are any. These two methods make no significant difference when the concurrency is low. However, when the concurrency is high, meaning many users are online simultaneously, the difference becomes substantial:

```mermaid
graph TD
subgraph Pull-Based Service
   Client1
   Client2
   Client3

  subgraph Service Processing
    Service1
    Service2
    Service3
end
end
Client1 --request--> Service1
Service1 --response--> Client1
Client2 --request--> Service2
Service2 --response--> Client2
Client3 --request--> Service3
Service3 --response--> Client3
```

Push-based service diagram:

```mermaid
graph TD
subgraph Push-Based Service
    A[Service Processing]
    B[Client]
    C[Client]
    D[Client]
end
A -- Push --> B
A -- Push --> C
A -- Push --> D
```

As shown above, with a pull-based service, each client sends a service request, such as outputting the latest 20 comments. The backend logic cluster processes each service request and queries the latest 20 comments from the database cluster. After fetching the data, it returns it to the requesting client. Each request needs to be processed by a separate thread. We can imagine that if 10,000 clients request simultaneously, the backend needs to open 10,000 threads to handle these requests. Conversely, with a push-based service, no matter how many clients there are, the backend computational service only needs to open one thread to query the data once from the database and push it to 10,000 clients. The connection service at the backend only forwards data, greatly reducing the service resource consumption compared to 10,000 service processing threads and 10,000 database requests.

**2. Strictly Avoid Requesting Large Amounts of Data During Case Initialization**

Sometimes we like to handle a lot of logic during case initialization, such as determining user permissions or obtaining initialization configurations. Handling high concurrency cases this way can easily lead to service blockage. When users find the case takes too long to load, they tend to refresh the case. The slower it opens, the more they attempt to refresh it. If the initialization service contains too much logic, it may easily cause timeouts.

Therefore, we need to ensure users can open the case smoothly. For example, having an initial page and introduction page that fetches content statically, and then when users need to participate in activities, they click a button to log in and other services. This effectively avoids the "initialization refresh hell."

**3. "Intercept" User Operations on the Frontend to Minimize the Number of Service Requests**

In many scenarios, we can fully utilize the computation resources of the client (frontend) to reduce the number of backend service requests. For example, for a multi-question quiz/test case that requires recording each user's answers, we can record the answers for each question on the frontend and submit them to the backend in one batch after all questions are answered, rather than submitting them to the backend after each question.

This approach essentially shifts the computation burden from the backend server to the client, utilizing client resources without any high concurrency limitations or costs.

**4. Add Indexes to the Database**

For high concurrency cases involving database queries, especially when the data volume is large, we must add indexes to the queried fields (i.e., fields used for filtering or statistical conditions). Adding indexes significantly reduces database query time, thereby lowering the computation time for each related database service.

**5. Strictly Avoid "Heavy" Database Operations**

The database is always the bottleneck in a large system. Although VisualLogic supports elastic scaling for databases, the scaling time for database instances is much longer than that for regular computing instances (10 to 20 minutes). This is because it involves data backup and a series of data security and monitoring registrations. Therefore, if a high concurrency case keeps requesting large amounts of data from the database, it can cause slow database responses and long service request times. When service request times are long, VisualLogic's service handling limits are triggered, leading to the server rejecting new service requests.

Hence, in high concurrency applications, we should minimize the burden on the database and avoid "heavy" database operations. "Heavy" database operations refer to those that cause long database computation times, commonly including:

* Outputting large amounts of data at once: VisualLogic's default limit is 50 rows. In high concurrency applications, we recommend not exceeding this limit. If displaying more than 50 rows of data, use pagination requests.
* Extensive data statistics: Includes the total, count, average, etc., of large amounts of data. For such statistical needs, we usually "exchange space for computation," i.e., create a statistics table and update it with statistical data each time the data updates, similar to the voting component logic. Use a candidate table to record each candidate's vote count in real-time, rather than calculating in real-time from the voting ledger.
 # Standalone Deployment 2: Keep the program running in the background.

### Standalone Deployment Method Two: Keeping Programs Running in the Background

This article outlines how to ensure a VisualLogic program continues running in the background when deployed on a single machine. We will discuss two methods, screen and service, to meet different technical and maintenance needs.

#### I. Simple Method: screen

`screen` is a program that allows you to open and manage multiple terminal windows simultaneously. We can use `screen` to ensure that the program running `VisualLogicbase` stays in the background. Here are the detailed steps:

1. **Check if screen is installed**  
   To check if `screen` is installed, use the following command:
   ```shell
   rpm -qa|grep screen
   ```
   If it's not installed, you can install it using:
   ```shell
   yum install screen
   ```

2. **Create a new screen session**
   After installation, use the following command to start `screen` and name each session for easy management:
   ```shell
   screen -S <yourname>
   ```
   Once run successfully, you enter the session.

3. **Run VisualLogic program**
   Run the `VisualLogicbase` program within the session. The program will continue to run in the background even if the terminal window is closed.

4. **Manage screen sessions**
   - List all sessions:
     ```shell
     screen -ls
     ```
   - Reattach to a specified session:
     ```shell
     screen -r <yourname>
     ```
   - Detach from the session but keep the program running:
     ```shell
     screen -d
     ```
   - Completely terminate the session:
     ```shell
     screen -S <yourname> -X quit
     ```

```mermaid
graph LR
A[Check if screen is installed] -->|Installed| B[Create a new screen session]
A -->|Not installed| C[Install screen]
B --> D[Run VisualLogic program]
D --> E[Manage screen sessions]
E --> F[List all sessions]
E --> G[Reattach to a specified session]
E --> H[Detach from the session but keep the program running]
E --> I[Completely terminate the session]
```

#### II. Professional Method: service

Compared to the `screen` method, `service` is more professional and easier for later maintenance. Here are the detailed steps:

1. **Create the configuration file**
   Navigate to the configuration file directory:
   ```shell
   cd /etc/systemd/system
   ```
   Create a configuration file named `visualtest.service`:
   ```shell
   sudo vim visualtest.service
   ```

2. **Edit the configuration file**
   Use `vim` to edit the following content:
   ```ini
   [Unit]
   Description=VisualLogic test
   After=network.target redis.service mariadb.service
   
   [Service]
   Type=simple
   User=celveta
   Group=celveta
   Restart=on-failure
   RestartSec=10
   WorkingDirectory=/home/celveta/visualtest
   ExecStart=/home/celveta/visualtest/VisualLogicbase
   
   [Install]
   WantedBy=multi-user.target
   ```
   Press `Esc` to exit `INSERT` mode and save and exit.

3. **Start the service**
   - Reload the configuration:
     ```shell
     sudo systemctl daemon-reload
     ```
   - Start the service:
     ```shell
     sudo systemctl start visualtest
     ```
   - Enable the service to start on boot:
     ```shell
     sudo systemctl enable visualtest
     ```

4. **View logs**
   ```shell
   sudo journalctl -u visualtest -f
   ```

```mermaid
graph LR
A[Create the configuration file] --> B[Edit the configuration file]
B --> C[Reload the configuration]
C --> D[Start the service]
D --> E[Enable on boot]
E --> F[View logs]
```

These two methods can help ensure that the VisualLogic program runs stably in the background. Choose the appropriate method for deployment and maintenance according to your specific requirements. The `screen` method is simple and quick, while the `service` method is better suited for long-term maintenance and management.
 # Object Storage (OSS)

### Enable OSS and Create a New Bucket

First, we need to enable Alibaba Cloud's OSS (Object Storage Service). The service is free to enable, simply search for "OSS".

Once enabled, go to the homepage and select to create a new bucket. Here, a bucket refers to a file container, which is the most basic unit of file storage. You can create multi-level folders within each bucket. Both Tencent Cloud and Amazon's object storage services also use buckets as their basic unit.

Then, set a bucket name, for example, visualtest, and choose a region that matches your server's location.

We don't need to modify other configurations, but temporarily set the default file access permission to public read, as we need other users to access our example resource files directly through the network.

After clicking confirm, we have a bucket.

### Create a Directory and Upload a Zip File

After creating the bucket, enter its file management interface.

Create a new directory (the name can be customized, just make corresponding entries in the config file later), for example, call it projects, to store all the VisualLogic cases for this test. Then, within the projects directory, create a new zip directory for storing the packaged zip files.

Next, export a VisualLogic case and upload the zip package to the zip directory. The interface operation of Alibaba Cloud OSS is similar to a regular online storage service, so detailed screenshots are not provided here. After uploading, you will see a file package under the zip directory.

In the above example, we implemented an image upload feature. Once deployed, we can see the files we uploaded directly in the OSS directory.

### Modify the Config File

Next, we need to modify the visualbase config file in two steps:

1. **Step 1: Modify the "loadPackage" Section**

After the bare-metal deployment in the previous step, a "loadPackage" section is automatically generated in the config.json:
Note that when using OSS, "dir" does not support "./", and a sub-folder must be specified:

```json
"loadPackage": {
    "type": "oss",
    "dir": "zip",
    "period": 1,
    "restart": true
}
```

Locate this section, and replace the previous default value with the above content:

- Change the "type" field from the default "file" to "oss", indicating the file source is OSS instead of local (even for Tencent Cloud or Amazon, this remains OSS as it refers to object storage services in general).
- Change the “dir” field from the default "./" root directory to the defined path, which in our example is "zip". Note that only "zip" is needed here, not "projects/zip", as "projects" is the designated working folder path, and only the sub-path of the designated working path is needed.

2. **Step 2: Add OSS Configuration**

Then, add an "oss" section in the config file to specify OSS-related configuration. Below are configuration details for Alibaba Cloud, Tencent Cloud, Amazon Web Services, Azure, Minio, which can be referenced based on the platform you are using:

- **Alibaba Cloud OSS**

```json
"oss": {
    "pathPrefix": "//visualtest.oss-cn-hangzhou.aliyuncs.com/projects",
    "provider": "aliyun",
    "endpoint": "oss-cn-hangzhou.aliyuncs.com",
    "accessKeyId": "LTAI1********mwZ",
    "accessKeySecret": "6x2Kk*****************dTcrhq",
    "bucket": "visualtest"
}
```

- **Tencent Cloud**

```json
"oss": {
  "pathPrefix": "//testvisual-1300978216.cos.ap-guangzhou.myqcloud.com/projects",
  "provider": "tencent",
  "endpoint": "cos.ap-guangzhou.myqcloud.com",
  "accessKeyId": "AKIDzmJZ*****************l8E4dvxlLgngIdh",
  "accessKeySecret": "oLbZHhq*****************QLzMSGo5",
  "bucket": "testvisual-1300978216"
}
```

- **Amazon Web Services (AWS)**

```json
"oss": {
    "pathPrefix": "//testvisual.s3.cn-northwest-1.amazonaws.com.cn/projects",
    "provider": "aws",
    "endpoint": "s3.cn-northwest-1.amazonaws.com.cn",
    "accessKeyId": "AKIAVL*****************22XYEXF",
    "accessKeySecret": "rw5eAE3Yw*****************i5aN8R1YdqvB",
    "bucket": "testvisual"
}
```

- **Azure**

```json
"oss": {
    "pathPrefix": "//visualtest.blob.core.chinacloudapi.cn/testvisual",
    "provider": "azblob",
    "endpoint": "chinacloudapi.cn",
    "accessKeyId": "visualtest",
    "accessKeySecret": "hSAwuZBQm45UUJcss2SLn/EKvOb*****************NEvqCKwDzrLWXazHlt7Xbvd0gqeAG6oxQ==",
    "bucket": "testvisual",
    "bucketInPath": true
}
```

- **Minio**

```json
"oss": {
    "pathPrefix": "http://visualtest.xxx/testvisual",
    "provider": "aws",
    "endpoint": "http://visualtest.xxx",
    "accessKeyId": "visualtest",
    "accessKeySecret": "hSAwuZBQm45UUJcss2SLn/EKvOb*****************NEvqCKwDzrLWXazHlt7Xbvd0gqeAG6oxQ==",
    "bucket": "testvisual",
    "bucketInPath": true
}
```

- **Huawei Cloud**

```json
"oss": {
    "pathPrefix": "https://visual.obs.cn-south-1.myhuaweicloud.com/projects",
    "provider": "aws",
    "endpoint": "http://obs.cn-south-1.myhuaweicloud.com",
    "accessKeyId": "WXHSY**********BSI1DIL",
    "accessKeySecret": "e2gsRJX**********0vFqff0D5oa77j9e",
    "bucket": "visual",
    "bucketInPath": true
}
```

Let’s explain the significance of each field:

- **pathPrefix**: The file path prefix. Since we haven't configured a CDN, use the file access address prefix provided by OSS directly. This prefix can be found in the basic bucket data. Copy this file prefix and point it to our working directory, appending “/projects”.
- **provider**: Service provider. Fill in based on your cloud service provider: aliyun, tencent, aws, azblob, or filesystem for local file systems. VisualLogic also supports the open-source locally deployable file storage system Minio; fill in "minio" here.
- **endpoint**: The regional node, obtainable from the basic bucket information.
- **accessKeyId and accessKeySecret**: These are the access ID and secret, obtainable from the cloud platform's permission management feature.
- **bucket**: The storage bucket name, which is the bucket we just created.

Once both sections have been modified, save the config file. Restart the service using the command to access our examples via OSS:

Restart the service:

```bash
sudo systemctl restart visualtest
```

Additionally, verify service status with the log view command:

```bash
sudo journalctl -u visualtest -f
```

After running successfully, check back in the OSS folder. In the projects work directory, visualbase automatically creates several additional files and folders. Among these, "files" contains all resource files in our example, including images, videos, and all user-uploaded files. The zip directory is currently empty because the previously uploaded zip package has been "consumed," i.e., parsed into executable files. We can upload a zip package again and restart the service to update our online example version.

```mermaid
graph LR;
    A[Enable OSS Service] --> B[Create New Bucket];
    B --> C[Set Bucket Name and Region];
    C --> D[Set File Access Permission to Public Read];
    D --> E[Create Projects Directory];
    E --> F[Create Zip Directory];
    F --> G[Export and Upload VisualLogic Case Zip Package];
    G --> H[Modify Config File];
    H --> I[Add OSS Configuration Section];
    I --> J[Restart Service];
    J --> K[Upload Complete];
```

### Attention: Cross-Domain Issues

When using OSS to store files, images under the canvas in the example may encounter cross-domain issues (images outside the canvas are not affected), as shown below:

To resolve this, enter the bucket settings and configure "Cross-Origin Settings." Create a rule, input an asterisk (*) to allow all sources to fetch images and solve the cross-domain issue. For detailed OSS cross-origin rules, refer to Alibaba Cloud's [official documentation](https://help.aliyun.com/document_detail/31903.html).
 # Configure Domain Name

## Configure Domain Name

In this section, we will introduce how to configure a domain name for our application to replace the previously used IP address. This includes domain name registration, DNS settings, and related configuration steps.

### Domain Name Registration and DNS Configuration

Before configuring the domain name, we need to first register a domain name and complete the necessary registration procedures. The specific steps are as follows:

1. **Domain Name Registration**: Visit the website of a domain service provider, such as Alibaba Cloud or Tencent Cloud, to select and purchase an appropriate domain name.
2. **Domain Registration Compliance**: Complete the domain registration compliance process as required by local regulations. Please refer to the guidelines provided by the domain registrar for specific operations.

After completing the domain registration and compliance procedures, we need to configure the domain name resolution.

### DNS Resolution Configuration

DNS resolution is used to translate the domain name into the server's IP address so that users can access the application via the domain name. The specific steps are as follows:

#### Add A Record Type

1. **Select Record Type**: Choose "A" to point the domain name to an IPv4 address.
2. **Set Domain Prefix**: Add a custom prefix to the domain name, e.g., `ihtest.ih5school.cn`.
3. **Fill in Server IP Address**: Enter the server's IP address, e.g., `39.104.21.197`.

Once the setup is complete, click confirm to finish the addition.

#### Example

For instance, using the domain `ih5school.cn`, we add a subdomain `VisualLogicTest`, pointing to our server's IP:

```
http://39.104.21.197:9900/Q&A -----> http://VisualLogicTest.ih5school.cn:9900/Q&A
```

After the DNS configuration is complete, wait for about 10 minutes to replace the previous IP access method with domain name access. It should be noted that after configuring the DNS, the previous IP access is still valid, except an additional method via the domain name is provided.

### Use DNS Service for Domain Name Resolution

As mentioned earlier, when a user enters a domain name, the browser requests the DNS (Domain Name System) to obtain the IP address corresponding to the domain name. Thus, we need to perform the following actions in the DNS configuration interface of the domain provider:

#### Add Record on Domain Provider Platform

Taking Alibaba Cloud as an example, the specific steps are as follows:

1. **Select your purchased domain and choose to add a record**.
2. **Select the record type as CNAME** to directly point to a default platform domain.
3. **Fill in the application subdomain**, such as `www` or `h5`, recommending all applications share a subdomain.
4. **Fill in the system release domain**, such as `v4rel.h5sys.cn`.

We can set an independent subdomain for each application or share one, using different access paths to differentiate. The latter is more recommended because many third-party API platforms require a legitimate business domain name.

#### Preview Domain Configuration (Optional)

If a custom domain name is needed during the preview, configure the preview domain resolution on the domain provider platform with the same method as the official domain resolution. Set different subdomains and point them to `v4pre.h5sys.cn`. Then, fill in the corresponding preview domain in the application configuration interface.

#### WeChat Official Account Domain Configuration (Optional)

Finally, if the application needs to run in the WeChat browser and access user information, a WeChat Official Account is required where the domain name configuration needs to be set.

### Flowchart

```mermaid
graph TD
    A[Domain Name Registration] --> B[Domain Registration Compliance]
    B --> C[DNS Resolution Configuration]
    C --> D[Select Record Type]
    D --> E[Set Domain Prefix]
    E --> F[Fill in Server IP]
    F --> G[Confirm Completion]
    G --> H[Wait for DNS Propagation]
```

By following the above steps, we have successfully replaced the IP address with domain name access, configured DNS resolution, and can perform preview domain and WeChat Official Account domain configurations when necessary.
 # Connect to cloud databases (RDS and Redis)

### Objective and Preparation

In this chapter, we will integrate our application with a cloud database. A cloud database is a database service managed by a third-party cloud platform. Similar to the previous chapters on OSS and CDN, the deployment in this chapter relies on the content of the standalone deployment chapter. However, unlike the deployment of OSS and CDN, it is independent. We can still host file resources on a local server, but the database will be integrated into a cloud database.

For commercial applications, if you don't have an existing mature local database solution, we strongly recommend integrating your application with a cloud database rather than using an on-premise database. This ensures stability and security of data access, eliminating concerns about database scaling and backup.

The database for VisualLogic applications relies on two fundamental databases: MySQL (a relational database) and Redis (a cache database). Here, we will continue to use Alibaba Cloud, utilizing the RDS service to integrate the MySQL database and the Redis version of the cloud database to integrate Redis.

### Create an RDS Instance

Firstly, find Alibaba Cloud's RDS (Relational Database Service), activate it, and create an instance:

1. Choose the instance specification according to your needs. Keep the following in mind:
   - The RDS instance needs to be in the same region as the compute server; otherwise, internal network access is unavailable.
   - Choose either MySQL or MariaDB for best compatibility.
2. Once the instance is created, you will see an instance being created in the instance list. The RDS instance creation takes longer than the server, approximately 10-20 minutes.
3. RDS is relatively costly, so during the testing phase, you can choose the smallest basic instance and assess requirements for an upgrade before commercial deployment.

### Configure the RDS Instance

Once the RDS instance is created, click on manage in the instance list to enter its management page. Before integrating with VisualLogic applications, the RDS needs to be configured, mainly involving two steps:

#### 1. Set Access Whitelist

In the basic information interface, set the access whitelist to allow the cloud server to access this RDS instance. Simply enter the internal IP of the cloud server, as RDS is accessed through the intranet.

#### 2. Create an Account

In the account management interface, select to create an account, set the username and password, and the cloud server will be able to access the RDS instance using this account. Additionally, in the cloud connection interface, you can directly view detailed information about the database using the account and password. Here, an account with the username "ivxtest" is created.

### Set config File

Finally, modify the database connection section in the config file, then restart the service:

```json
"database": {
    "driver": "mysql",
    "addr": "rm-hp317p06rgw9s2w3j59760.mysql.huhehaote.rds.aliyuncs.com:3306",
    "user": "ivxtest",
    "passwd": "xxxxxx",
    "dbName": "test"
}
```

- `addr`: The RDS access address, which can be found in the basic information of the RDS instance. Remember to add the port, default is 3306.
- `user`: The username created in the previous step.
- `passwd`: The password created in the previous step.
- `dbName`: Customizable, it determines which library to create tables in your application. One RDS instance can contain multiple libraries; here, a library named "test" is created.

After saving the config file, restart the service to migrate the database to RDS.

### Validate Configuration

To verify that the project is running on the cloud database, connect to the database remotely using the previously set account and password. You will find that all tables and all pre-configured data for the project have been created in the cloud database.

### Integrate Redis

Next, proceed to integrate the Redis service.

1. Similar to RDS, create an Alibaba Cloud Redis instance in the region where the server is located. The creation process is almost the same as for RDS, so it will not be demonstrated again.
2. After the Redis instance is created, obtain the internal access address and port from the basic information page.
3. Configure the Redis instance's access password and whitelist, and finally, in the redis section of the config file, fill in the corresponding address and access password:

```json
"redis": {
    "addr": "r-hp3obf34u63ihswvrc.redis.huhehaote.rds.aliyuncs.com:6379",
    "passwd": "XXXX"
}
```

### Full Process Diagram

```mermaid
graph LR
    A[Find RDS Service] --> B[Activate RDS Service]
    B --> C[Create RDS Instance]
    C --> D[Set Access Whitelist]
    D --> E[Create Account]
    E --> F[Modify config File]
    F --> G[Restart Service]

    subgraph Configure Redis
        H[Find Redis Service] --> I[Create Redis Instance]
        I --> J[Set Access Password and Whitelist]
        J --> K[Modify config File]
        K --> L[Validate Configuration]
    end
```

Through this process, cloud database RDS and Redis integration and configuration can be achieved effortlessly, providing stable, secure, and efficient database support for VisualLogic applications.
 # Configure CDN

## Objectives and Preparation

In this chapter, we will configure our application for a CDN service. CDN stands for "Content Delivery Network," designed to distribute our files across various server nodes nationwide or globally, ensuring users from different locations can quickly access the files. Typically, a formal commercial use case requires integrating a CDN service to guarantee access speeds for users everywhere.

To connect to a CDN service, we can use our logical server directly as the file source or a cloud object storage service. Here, continuing from the previous session's OSS deployment, we integrate Alibaba Cloud's CDN service with our OSS bucket.

It is essential to note that we use Alibaba Cloud’s CDN solely for ease of integration and deployment. For CDN services, we can choose any provider that offers reasonable costs and stable access. For instance, if we use Alibaba Cloud servers or OSS, we can integrate with Tencent Cloud or Qiniu Cloud's CDN, and vice versa. CDN service is relatively independent and a "homogeneous" service, allowing us to configure multiple CDN providers redundantly and switch in real time via DNS domain resolution.

Although we have no specific requirements for the CDN service provider, we strongly recommend using an object storage service (like Alibaba Cloud's OSS, Tencent Cloud's COS, or Amazon/Huawei Cloud's S3) to integrate with the CDN service rather than directly using the server's local file storage. This recommendation stems from the need for CDNs to perform a "back-to-source" process, fetching or updating files from the file source. This process can significantly increase requests to the server, potentially impacting service provision if the CDN pulls directly from the server.

## Enabling CDN Service and Configuring Domain

Next, building upon previous session results, we integrate the CDN service. First, we access Alibaba Cloud’s CDN service and enable it. Upon enabling, we can select the payment mode based on actual usage scenarios, choosing "pay-as-you-go" here.

After enabling, we need to add a domain:

### Domain Configuration Steps

1. **Add CDN Domain**
   - CDN Domain: The file prefix used after integrating with the CDN. A registered domain is required, with permission to configure DNS resolution, e.g., cdn.ih5school.cn.
   - Origin Information: The file source needing acceleration. Here, select Alibaba Cloud's OSS and choose the previously created `VisualLogic` bucket.
   - Port: Choose 443, as HTTPS access is needed.
   - Acceleration Region: Choose based on demand, selecting domestic access here.

2. **After filling in the information, click to add, completing domain addition:**
3. **Return to the domain list to see the newly added domain under configuration, indicating "CNAME not configured":**
4. **Proceed to your domain's resolution service to add a CNAME record pointing to the specified address** Refer to on-page help documents if unsure how to configure CNAME domain resolution.
5. **Wait for DNS resolution to take effect and for CDN domain configuration to succeed**—approximately 10-15 minutes before refreshing the list to see CDN fully configured.

```mermaid
graph TD
    A[Enable CDN Service] --> B[Add Domain]
    B --> C[Fill in Required Fields]
    C --> D[Complete Domain Addition]
    D --> E[Return to Domain List]
    E --> F[Configure CNAME]
    F --> G[Wait for DNS to Take Effect]
    G --> H[Refresh List]
    H --> I[CDN Configuration Complete]
```

## Enabling HTTPS

Next, we need to enable HTTPS certificates for the CDN service. Without enabling HTTPS, cases involving HTTPS access with internal files supporting only HTTP will encounter cross-origin issues, preventing file loading. Enabling HTTPS is therefore essential.

In the prior session, the CDN domain operated normally, but without HTTPS enabled:

1. **Click the management button on the right to open the management interface and access the HTTPS certificate configuration:**
2. **In the configuration interface, enable HTTPS and choose a free certificate:** Most CDN providers offer free certificates sufficient for most scenarios, although certificates have validity periods—Alibaba Cloud's free certificate is valid for one year, requiring manual renewal upon expiration.
3. **After confirmation, wait for certificate issuance and effectiveness**—a process that could take minutes to over ten minutes, refreshing the list to see HTTPS enabled.

## Modifying the Config File

With the CDN service configured, only the final step remains to enable our case to use CDN acceleration.

Access the config file, changing the `"pathprefix"` under the `"oss"` field from the previous OSS address to the CDN address:
```json
"oss": {
    "pathPrefix": "//cdn.ih5school.cn/projects",
    "provider": "aliyun",
    "endpoint": "oss-cn-hangzhou.aliyuncs.com",
    "accessKeyId": "LTAI*******vmwZ",
    "accessKeySecret": "6x2Kk*********JdTcrhq",
    "bucket": "VisualLogic"
}
```
Note: Replace the OSS domain with the new CDN address (`//cdn.ih5school.cn`), using a relative path for automatic HTTP or HTTPS usage based on the case address, and retain the working directory path, i.e., `/projects`.

After modifying the config file, restart the service and refresh the case to complete CDN service integration.

Finally, review the command to restart the service:
```
sudo systemctl restart VisualLogic
```"
 # Overview of Private Deployment Function

## # Function Overview

The VisualLogic platform provides a feature for private application export and deployment, supporting the exportation of application code to be deployed on your own server. Users can choose to export either front-end code only or both front-end and back-end code together. After export, users can deploy the application on a standalone server or a distributed network of automatically expandable servers or containers (such as Docker). Additionally, third-party cloud services, including CDN, object storage (OSS/S3), cloud databases (RDS), etc., can be optionally integrated.

Whether it is a purely front-end package or an integrated front and back-end package, once exported and deployed, the application will be completely independent of the VisualLogic platform. This means the database and media resources are unrelated to the VisualLogic platform. Consequently, the exported and deployed application does not incur any server computation or traffic costs on VisualLogic, but users will need to maintain their own servers.

## # Application Deployment Architecture

A VisualLogic application at runtime relies on the following backend modules:

1. **Network Services**: Network service programs such as reverse proxies. For private deployment, users can use their own Nginx, Apache programs, or cloud service platform load balancing services.
2. **Computing Units**: Core backend computing program of VisualLogic, i.e., the base file downloaded during application export. This computing program can be deployed on a standalone server, elastically scaled across multiple servers, or run in container services.
3. **Object Storage**: File storage services, which can be local file storage on the server or cloud platform's object storage services such as Alibaba Cloud's OSS, Tencent's COS, and Amazon's S3.
4. **MySQL**: MySQL relational database, which can be a locally installed MySQL program or cloud platform's RDS service.
5. **Redis**: Redis cache database, supporting local installation and cloud resources.

These service modules are essential for the operation of a VisualLogic application, but the deployment method is not restricted. Users can select the deployment architecture based on their actual needs.

A minimal cost deployment architecture involves deploying all five modules on a single server. For distributed deployment, supporting elastic scaling of computing units, network services, and file/database services must be independently deployed to ensure uniform data service access by multiple computing units.

## # Application Package Structure

Via the application export interface, users can export the runtime code of the current VisualLogic application. The structures of two export package types are as follows:

### # 1. Integrated Front-end and Back-end Package

Packages the entire application for deployment on your own server. The integrated package contains two parts:

- **Base Program Files**: Core system code for the backend, supporting Linux, Windows, Mac, and Linux Docker image formats. Once running, the base program can support multiple applications and seamless version updates. Unless significant system updates occur, updating the base program is typically unnecessary.
- **Application Code Package**: Current application code, including both front-end and back-end parts. Once downloaded to the specified location, the base program will automatically parse and run the application. After updates, users can download the application package again to the specified location, and the base program will update the application without interrupting services.

### # 2. Front-end HTML Package

A package exporting only the front-end part of the application. This can run directly on a local computer via a browser or be deployed to a cloud server along with self-hosted web services. VisualLogic also supports exporting front-end ads in formats for Facebook, Google, and Pangle, which can be uploaded directly to corresponding ad platforms. The front-end package format is a ZIP file containing all front-end code and resource files, with the application accessed directly via the index.html file.

## # About Source Code Export and Secondary Development

Currently, VisualLogic exports compiled deployment packages rather than source code packages. These packages can be deployed and run directly but do not support secondary development by modifying the exported source code. The lack of source code modification support is to prevent discrepancies that arise from reimporting modified source code back into VisualLogic for editing. To maintain consistency in the application development workflow, it is strongly recommended to make modifications and iterations within the VisualLogic platform.

By utilizing custom functions, custom components, and API/RPC interfaces, VisualLogic supports open development. Developers can connect to third-party resource libraries or use code mode development directly within the platform, eliminating the need for source code modifications for secondary development.

## # Recommended Configuration for Private Deployment

### # Application Private Deployment

For applications exported to privately managed servers, the following configurations are recommended:

- Standard server configurations are sufficient without special requirements.

### # Tools Private Deployment

To deploy the entire VisualLogic development platform (including the editor, workspace, etc.) within an enterprise's internal environment, the following configurations are recommended:

- Medium or higher enterprise-grade server configurations.
- As CentOS is no longer maintained, it is recommended to choose OpenEuler 22.03 version to better align with domestic localization and innovation requirements.

## # Enterprise Private Deployment Solutions

The VisualLogic platform's private deployment allows enterprises to deploy the entire system onto internal designated servers. Additionally, VisualLogic supports custom services, such as unified enterprise authorization or single sign-on (SSO), or integrating the generated code into the existing enterprise code management system. Enterprises can also customize the application's theme, replace the corporate Logo, etc., to meet specific branding and needs.

## # Deployment Flowchart

```mermaid
graph LR
A[Confirm Requirements]
B[Select Export Package Type]
C[Download Base Program]
D[Download Application Code Package]
E[Deploy Base Program]
F[Upload Code Package]
G[Start Application]
A --> B
B -->|Select Integrated Package| C
B -->|Select Front-end HTML Package| D
C --> E
D --> F
E --> F
F --> G
```"
 # Load Balancing and Auto Scaling

## # Objectives and Preparation

In this chapter, we will configure the load balancing and auto-scaling services for our private VisualLogic application. The purpose of these services is to handle high application traffic, where a single server cannot meet demand and more servers are required to process high concurrent computation requests. Load balancing and auto-scaling are used together: the load balancer is responsible for receiving service requests at the front-end and distributing them to the backend servers in the auto-scaling group; the auto-scaling group automatically scales new servers according to predefined rules when server load is high and releases excess servers when the load decreases to save costs.

To utilize an automatically scalable server architecture, we're required not only to complete the basic bare-metal deployment but also to deploy files and databases onto the cloud. This is necessary because deploying files and databases on a compute server would prevent automatically created servers from accessing the files and databases originally stored on that server. The content in this chapter relies on previous chapters on single-machine deployment, Object Storage Service (OSS), and Relational Database Service (RDS), but since the load balancer automatically takes on the role of a reverse proxy, we do not need to manually deploy an Nginx reverse proxy.

## # Creating Load Balancer

### # 1. Create Instance

First, locate Alibaba Cloud's Server Load Balancer (SLB) service and create an instance. When creating an instance, ensure the instance's region matches the region of your cloud servers.

### # 2. Configure Load Balancer

(1) Select protocol and port for external access:

- 443 is the default port for HTTPS. To additionally support HTTP access, you can add an HTTP listening port, default being 80.
- A load balancer supports multiple listening ports.

(2) Select certificate:

- HTTPS requires a certificate. You need to apply for a certificate with Alibaba Cloud's SSL Certificate Service and select the applied certificate.

(3) Configure backend servers:

- Select the default server group, then within the "Continue adding" entry, choose the previously configured server.
- Fill in the service port, usually 9900, with a default weight of 100 (currently with only one server).

(4) Complete configuration:

- Choose whether to enable health checks, leaving the default option enabled.

After completing the configuration, you can see the confirmation interface.

### # 3. Configure Domain Name

Once the load balancer is created, you can view its public IP in the management interface. Next, in the DNS service, point the domain name bound to the certificate to this IP, allowing application access via the domain name.

## # Configuring Auto-Scaling

### # 1. Create Scaling Group

Locate Alibaba Cloud's Auto Scaling service and create a scaling group.

### # 2. Select Template Instance

Choose the source configuration as an existing instance, then select the previously configured server.

### # 3. Set Maximum and Minimum Instance Numbers

Specify the range of instance numbers in the scaling group, with the minimum value typically being 1.

### # 4. Choose Associated Load Balancer and RDS

Select the associated load balancer and RDS for the scaling group to ensure new servers can access RDS. Once associated with an RDS instance, the scaling group will automatically register new servers upon expansion, ensuring RDS access.

After the configuration is complete, when the auto-scaling conditions are met, you will see automatically created servers in the backend server group of the load balancer.

## # Limitations of Auto-Scaling

Although the auto-scaling service can automatically add server resources during high concurrency, there are still some limitations:

1. **Auto-Scaling Takes Time**: The process involves resource monitoring, instance creation, health checks, and service registration, usually taking several minutes. During a sudden traffic surge, auto-scaling may not respond in time. It is advised to increase the minimum number of servers in the scaling group before major events.

2. **Auto-Scaling is Only for Compute Servers**: Most database services cannot scale elastically, such as RDS. Therefore, even if frontend compute servers can auto-scale, database access may still become a bottleneck. You should prepare for larger RDS resources or use databases with better scaling, like PolarDB, and perform performance testing on the database in advance.

```mermaid
graph TD
    A[Create Instance]
    B[Configure Load Balancer]
    C[Select Protocol and Port]
    D[Select Certificate]
    E[Configure Backend Servers]
    F[Enable Health Check]
    G[Configure Domain Name]
    H[Create Scaling Group]
    I[Select Template Instance]
    J[Set Maximum and Minimum Instance Numbers]
    K[Associate Load Balancer and RDS]
    L[Complete Configuration]

    subgraph Creating Load Balancer
        A --> B --> C
        B --> D
        B --> E
        B --> F
    end

    subgraph Configuring Auto-Scaling
        H --> I
        H --> J
        H --> K
        H --> L
    end

    A --> G
    C --> G
    D --> G
    E --> G
    F --> G
```"
 # Container deployment (Docker)

## # Objectives and Preparations

In this chapter, we will briefly introduce how to deploy VisualLogic applications to a container service. Containers (Docker) are finer-grained virtual computing units compared to servers, offering better resource utilization efficiency and scalability. Therefore, containerized deployments are increasingly prevalent in large-scale enterprise application deployments.

To run VisualLogic applications in containers, we must prepare the following resources in advance:

- Object Storage (OSS/S3)
- Cloud Database RDS
- Cloud Database Redis

Since containers are stateless (i.e., they do not retain application data) pure computational services, we need to integrate shared file and database services.

## # Program Export and Upload

Currently, the Docker deployment of VisualLogic applications runs in the form of a base image file and application resource package. In the application export interface, you can export the base image and the application code package separately.

### # Base Image Upload

After downloading the base image file, it needs to be pushed to the respective image repository using Docker Push commands. For creating a Docker cluster and pulling images, you can refer to the documentation of each cloud platform. For example, you can refer to Alibaba Cloud’s image service operations here.

### # Application Code Package Upload

The application code package needs to be uploaded to a public object storage service, such as Alibaba Cloud’s OSS. See the previous Object Storage chapter for details. The method of uploading files here is the same as when integrating object storage during ordinary server deployment. You just need to enable the object storage service and fill in the access address and authorization password of the object storage in the Docker related configuration.

## # Configurations

Different from ordinary server deployments, in container deployments, the configuration of the VisualLogic base is not stored in a configuration file but is passed in externally via an environment variable.

The environment variable name for the VisualLogic base is:

`CONFIG_PATH`

The VisualLogic base program will automatically look for the environment variable named "CONFIG_PATH" to obtain configuration-related information. Therefore, we need to specify the data source of this environment variable in the configuration of the container workload (deployment).

Taking Alibaba Cloud’s K8S container service as an example, we first need to create a configuration item, say, called config:

Then store the JSON-formatted information from the previous config file in a customized key (for example, "myconfig"):

Note that in this configuration, we need to specify the addresses of OSS, MySQL (RDS), and Redis databases. These addresses must be public service addresses accessible by the current container.

Then, in the data volumes of the container workload (deployment), add a local storage item of the configuration item type and select the newly created configuration as the mount source (in the above example, it is "config"):

Here, you need to additionally define a container path, which can be completely customized. Here, it is filled as "/ivx".

Finally, in the environment variable configuration, specify the path of the corresponding configuration information:

Each cloud platform has different methods to add container configurations. However, you just need to upload the configuration JSON, create an environment variable named "CONFIG_PATH", and point it to this configuration JSON.

### # Deployment Flowchart

```mermaid
flowchart TD
    A[Prepare Resources] --> B[Export Base Image]
    B --> C[Export Application Code Package]
    C --> D[Upload Base Image]
    D --> E[Upload Application Code Package]
    E --> F[Create Configuration Item]
    F --> G[Add Data Volume]
    G --> H[Define Container Path]
    H --> I[Configure Environment Variable]
```

By following the above steps, you can successfully deploy VisualLogic applications to Docker containers, ensuring efficient operation and full utilization of resources.
 # Extra: Windows + Apache Deployment

### 1. Export and Install the Base Platform

**Install the Base Platform:**

1. Click the "File" menu in the upper left corner of VisualLogic IDE and select "Export Application Package - Export Server Deployment Base - Windows Version."
2. After downloading, upload the file to the directory of the custom domain on your server.
3. Register a port (do not use port numbers 80 or 443).

**Import Official Redis and MySQL to Domain Directory**

- If there is an existing MySQL on the server, you can install Redis separately.

**i. Install Official Redis and MySQL Package**

```
Detailed steps for downloading and installing Redis and MySQL
```

**ii. Install Redis Separately**

- GitHub download link: [https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)
- Baidu Cloud download link: [https://pan.baidu.com/s/1z1_OdNVbtgyEjiktqgB83g](https://pan.baidu.com/s/1z1_OdNVbtgyEjiktqgB83g) Password: kdfq
- Place the extracted Redis folder into the domain directory.
- Open the `redis.windows.conf` configuration file, search for `requirepass`, comment out `requirepass foobared`, and add a password:

  ```
   # requirepass foobared
  requirepass 123456  // Note: No spaces at the beginning of the line
  ```
- Register Redis as a service:

  - Navigate to the Redis installation directory
  - Shift + Right-click to open the menu and select "Open Command Window Here" to open cmd.
  - Install Redis service:

    ```
    redis-server --service-install redis.windows.conf --service-name redis --loglevel verbose
    ```
  - Start Redis service:

    ```
    redis-server --service-start --service-name redis
    ```
- It is recommended to install a visualization database management tool like Navicat for MySQL.
- In the case of an existing MySQL database:

  - Create a new user and password in MySQL.
  - Create a new database and set the user's permissions for the new database.

**Install the Base Platform**

1. Double-click `ivxbase.exe` and enter relevant configurations including port number, Redis password, MySQL username, password, database name, etc. Typically, other addresses and IPs use default values.
2. After successful installation, open `config.json` to check if the configuration was successful.
3. Drag the case package to the domain directory (the case package must be in the same directory level as `ivxbase.exe`), if `ivxbase.exe` is already running, it will automatically install the deployment package. If there is no response, try restarting `ivxbase.exe` (ensure Redis is running).
4. After successful installation, enter `127.0.0.1:your_port_number/play/xx_your_case_path` in the server browser to open the case. The `/play/` path is set and modified in the VisualLogic editor configuration.

After completing domain and public configuration, you can access it using `domain:port_number/path`.

### 2. Reverse Proxy and Real IP Passthrough

Configuring a reverse proxy allows direct pointing to the local VisualLogic port of the server when using links without port numbers (http://xxx.com/play/aabb), and configures real IP passthrough; otherwise, these links cannot successfully call WeChat component functions.

**a) Public Configuration Modification**

1. Open `httpd.conf` file (generally located in the `Apache24/conf` folder), find the following authorizations, and remove the `#` in front:

   ```apache
   LoadModule proxy_module modules/mod_proxy.so
   LoadModule headers_module modules/mod_headers.so
   LoadModule proxy_html_module modules/mod_proxy_html.so
   LoadModule proxy_http_module modules/mod_proxy_http.so
   LoadModule proxy_http2_module modules/mod_proxy_http2.so
   LoadModule xml2enc_module modules/mod_xml2enc.so
   ```
2. If unable to redirect to the proxy port, add the following at the bottom of the `httpd.conf` file:

   ```apache
   <IfModule http2_module>
     ProtocolsHonorOrder On
     Protocols h2 h2c http/1.1
   </IfModule>
   ```

**b) Reverse Proxy Configuration (Choose One)**

**i. Global Reverse Proxy (Applicable if the server hosts only one domain solely for VisualLogic use)**

Add the following to the `httpd.conf` file:

```apache
<Location "/">
   ProxyPass "http://127.0.0.1:your_port_number/"
   ProxyPreserveHost on
   RequestHeader set X-Real-IP "expr=%{REMOTE_ADDR}"
   RequestHeader set X-Forwarded-Proto "expr=%{REQUEST_SCHEME}"
</Location>
```

**ii. Multi-Domain Server, Reverse Proxy Only for Specific Domain (Requires Changes to Two Files)**

1. Modify the `Apache24/conf/extra/httpd-vhosts.conf` file, find the domain location, and add:

   ```apache
   <VirtualHost *:80>
     ServerName xxxx.com   # Your domain
   
      # Insert the following code
     ProxyRequests off
     ProxyPreserveHost On
     ProxyPass / http://127.0.0.1:your_port_number/   # Change to your port number
     ProxyPassReverse / http://127.0.0.1:your_port_number/
   </VirtualHost>
   ```
2. Modify the SSL file corresponding to the domain (inside `Apache24/conf/extra` folder). For a single-domain server, it is generally `httpd-ssl.conf`; for multi-domain servers, each domain has a corresponding `conf` file, find the `conf` file for the current domain.

   Add the following at the top of the file:

   ```apache
   LoadModule xml2enc_module modules/mod_xml2enc.so
   ```

   In the `<VirtualHost *:443>` node, add:

   ```apache
   <VirtualHost *:443>
      # On the basis of the original code, add the following code
     SSLEngine On
     SSLProxyEngine On
     SSLProxyVerify none
     SSLProxyCheckPeerCN off
     SSLProxyCheckPeerName off
     <Location "/">
       ProxyPass "http://127.0.0.1:your_port_number/"
       ProxyPreserveHost on
       RequestHeader set X-Real-IP "expr=%{REMOTE_ADDR}"
       RequestHeader set X-Forwarded-Proto "expr=%{REQUEST_SCHEME}"
     </Location>
   </VirtualHost>
   ```

**c) Restart Apache Service**

Restart Apache in Task Manager - Services.

**d) Test Access Link**

Use `http(http)s://xxxx.com/play/aabb` to access without port number.

**e) Apache Unable to Start Troubleshooting**

Check whether the following permissions are enabled:

```apache
LoadModule actions_module modules/mod_actions.so
LoadModule alias_module modules/mod_alias.so
LoadModule allowmethods_module modules/mod_allowmethods.so
LoadModule asis_module modules/mod_asis.so
LoadModule auth_basic_module modules/mod_auth_basic.so
LoadModule authn_core_module modules/mod_authn_core.so
LoadModule authn_file_module modules/mod_authn_file.so
LoadModule authz_core_module modules/mod_authz_core.so
LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
LoadModule authz_host_module modules/mod_authz_host.so
LoadModule authz_user_module modules/mod_authz_user.so
LoadModule autoindex_module modules/mod_autoindex.so
LoadModule cgi_module modules/mod_cgi.so
LoadModule dir_module modules/mod_dir.so
LoadModule env_module modules/mod_env.so
LoadModule headers_module modules/mod_headers.so
LoadModule include_module modules/mod_include.so
LoadModule isapi_module modules/mod_isapi.so
LoadModule log_config_module modules/mod_log_config.so
LoadModule mime_module modules/mod_mime.so
LoadModule negotiation_module modules/mod_negotiation.so
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_html_module modules/mod_proxy_html.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule proxy_http2_module modules/mod_proxy_http2.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule ssl_module modules/mod_ssl.so
LoadModule xml2enc_module modules/mod_xml2enc.so
```

**f) Check SSL Configuration**

If the above configuration is correct but the https link still cannot be accessed, please check again if the SSL file is correctly referenced.

```mermaid
graph TD;
    A[Export Application Package] --> B[Transfer to Server Directory]
    B --> C[Register Port]
    C --> D[Import Redis and MySQL]
    D -.->|Existing MySQL| E[Install Redis Separately]
    E --> F[Register Redis as Service]
    F -.-> G[Install Navicat for MySQL]
    G --> H[Create User and Permissions]
    H --> I[Double-Click to Install Base Platform]
    I --> J[Open Configuration File to Check]
    J --> K[Drag in Case Package]
    K --> L[Restart ivxbase.exe]
    L --> M[Access Case in Browser]
```"
 # Limitations and Cognitive Misconceptions

## # Limitations of VisualLogic Development

VisualLogic is a new type of graphical programming language that comes with its own IDE, capable of generating full-stack code, compiling and running independently, and deploying to various servers or cloud platforms. Although VisualLogic has high flexibility and wide applicability, it does have some specific limitations, especially under current technical conditions.

The main limitations of VisualLogic lie in the development of large-scale 3D games and complex algorithms. For example, VisualLogic may not be mature enough in handling massive 3D models, and algorithm complexity remains a challenge for VisualLogic. Currently, graphical programming languages hold their ground against textual programming in terms of logical expression. VisualLogic leverages object-oriented component design theory, abstracting everything in the world as "components," theoretically enabling it to develop any application suitable for various development scenarios.

## # Misconceptions about VisualLogic

During the use of VisualLogic for development, several misconceptions frequently arise, including:

- **Comparison with SaaS**: Many users tend to compare VisualLogic directly with SaaS, evaluating which is better. In reality, VisualLogic can be used to develop various SaaS products, but fundamentally, it is a more underlying language and tool, not directly comparable to SaaS.
- **Changes in Operational Habits**: As VisualLogic adopts a graphical development process, differing from traditional coding methods, new users might need time to adapt. Particularly, VisualLogic is continually optimizing the utilization of external libraries and the gradual construction of an open-source ecosystem.
- **Adjustment of Learning Expectations**: Although VisualLogic lowers the difficulty of learning programming, time is still required to master the tools and architecture. With increased proficiency, development speed will significantly improve.

## # Advantages and Positioning of VisualLogic Development

VisualLogic fundamentally differs from other low-code platforms. It not only facilitates application generation within the platform but also provides the feature of exporting complete source code, allowing developers to exit the no-code development environment and revert to familiar textual coding modes whenever necessary. This reversibility endows VisualLogic with immense technical flexibility, averting technological lock-in.

VisualLogic is more than just an enterprise-level service platform; it is designed for developers worldwide. Through supporting graphical programming and code generation, VisualLogic ensures a high degree of programming freedom, extensively covering various enterprise application development scenarios.

## # Usage Limitations

In the deployment of enterprise or customized versions of the VisualLogic system, users may face the following limitations:

1. Limit on the number of users, including sub-accounts and main accounts.
2. Limit on the number of applications.
3. Usage period limitations.

These restrictions may vary depending on the deployment plan you choose.

## # Additional Content

**Flexibility and Limitations**
VisualLogic's flexibility is almost unrestricted, allowing developers to freely choose frontend or backend frameworks. However, due to less abstraction and encapsulation of components, VisualLogic is temporarily unsuitable for complex algorithm design. Nonetheless, from an overall development framework perspective, VisualLogic, with its high flexibility and rich component ecosystem, suffices various application development needs.

**Development and Application Scenarios**
VisualLogic is suitable for almost all application development scenarios, supporting multiple operating systems and platforms. However, for classical algorithms and certain highly specialized computation tasks, VisualLogic may still require further technological development and adaptation.

Typical applications developed with VisualLogic include enterprise management systems, e-commerce platforms, 2D/3D games, and solutions spanning nearly all industries, from satellite manufacturing to smart city management.

## # Conclusion

As a revolutionary graphical programming language, VisualLogic is transforming traditional software development paradigms through visual component construction and efficient code generation. It not only meets enterprise service needs but also, through its flexible componentized design, no-code development vision, and high code quality, offers developers infinite possibilities.

Those with a positive mindset will grow amid technological change, whereas pessimists might find it hard to adapt to rapid technological advancements. Hence, understanding the core value of VisualLogic and its industry applications is particularly crucial. Continuously exploring the conveniences offered by these graphical programming languages will help maintain a leading position in technological innovation.
 # VisualLogic Code Generation Process and How to Perform Secondary Development

## # VisualLogic Code Generation Process

VisualLogic is a general-purpose graphical programming language that adopts a component-oriented programming paradigm to rapidly build applications. VisualLogic abstracts various front-end and back-end "atomic components," which are designed to be object-oriented, possessing "atomic" properties capable of constructing any type and function of applications. Each atomic component includes three elements: "trigger conditions," "attributes," and "functions."

### # Logic Panel and Abstract Syntax Tree (AST)

VisualLogic provides developers with a powerful "Logic Panel," a core tool that implements application logic without writing any code. The Logic Panel combines various atomic components and functional logic to form an "Abstract Syntax Tree (AST)." This process is a core step in application development. Though VisualLogic removes syntactic barriers, developers still need to independently construct the logic. Through learning and practice, developers can usually proficiently build various logic for common applications.

### # Code Generation

Finally, VisualLogic, through its dedicated compiler, supports generating specific code for the application logic, currently mainly supporting JavaScript and SQL generation. The VisualLogic Logic Panel is "Turing complete," meaning it can theoretically express any reasonable logic, allowing VisualLogic to achieve no-code development.

## # Code Execution Environment

- **Front-end Code**: Can generate JavaScript or WeChat Mini Program code to adapt to the corresponding system environment.
- **Middleware Code**: Mainly generates JavaScript code and runs in server environments like Deno, supporting multithreading execution. If using VisualLogic cloud-hosted applications, these codes will be packaged into Docker and run in Lambda function computing.
- **Database Interaction**: The SQL statements generated by VisualLogic are executed in databases like ElasticSearch, MySQL, etc.

Additionally, VisualLogic uses Go language-written DLLs (Dynamic Link Libraries) mainly to handle cloud computing IaaS layer resource access that doesn't involve application business logic.

## # Secondary Development

Secondary development on VisualLogic is considered an iterative extension of the initial development. Developers can seamlessly extend and export applications for privatized deployment directly on VisualLogic. Although VisualLogic allows direct modification of the generated JavaScript code or even embedding custom functions, it is strongly recommended to avoid changing auto-generated code, as such modifications may result in the inability to re-import into the system.

## # Summary

With its no-code programming, component-oriented design, flexible Logic Panel, and powerful code generation capabilities, VisualLogic significantly improves application development efficiency. Whether for initial development or subsequent iterations, VisualLogic provides a stable and efficient platform for developing and maintaining applications. Despite the flexibility of embedding code directly, developers usually can achieve the desired functionality without writing any code, making VisualLogic a tool suitable for both programming novices and experienced programmers.
 # Background and Development

### # Background and Motivation for VisualLogic Development

The development of high-level programming languages has spanned more than half a century since their inception. However, the core way of programming has not undergone a fundamental transformation, remaining at the "coding" stage. In response to this situation, VisualLogic emerged. VisualLogic aims to explore a new path through graphical programming to "smooth out" the differences between various programming languages or frameworks, achieving "removing program syntax, retaining program logic." This new approach seeks to simplify the entire process from development, testing, to deployment and operations, and can automatically generate full-stack code, providing users with the most intuitive way of programming.

The concept of graphical programming was proposed decades ago, leading to a series of products such as Visual Basic and Dreamweaver. However, these products still have many shortcomings in realizing the graphical expression of core logic and have not entirely addressed the complexity issues in programming. VisualLogic is committed to solving these technical challenges through a new component-oriented programming approach.

### # Challenges and Key Innovations in Graphical Programming

**Innovation in Software Engineering**

In traditional software development, despite the emergence of theories such as OOP, Agile, and Lean Development, the core development paradigm has not fundamentally changed. Traditional programming methods, due to their complexity, are not beginner-friendly and have become an obstacle to expanding "software" productivity.

A new programming language has the value of achieving this goal if it can surpass traditional code programming in at least three of the following four areas: higher learning efficiency, faster development efficiency, easier maintenance, and faster runtime speed.

**Revolution in Underlying Technology**

A crucial aspect of creating a graphical programming language like VisualLogic is designing the flexibility of the product architecture to quickly adapt to technological iterations. The conversion between graphical programming languages and existing programming languages is also a significant challenge. VisualLogic has successfully implemented rapid conversion between different programming languages by redesigning the abstract syntax tree (AST) and other methods.

**Innovation in Product Design**

From its early product design stages, VisualLogic faced major challenges, expanding from supporting basic front-end development to include back-end, database development, cloud computing, and software lifecycle management. This required continuous investment in product iteration and technology exploration by the team, along with independently developing many innovative solutions.

### # The Birth and Outlook of VisualLogic

VisualLogic was developed over seventeen years, achieving online access from 2021 to 2022. After 7 to 8 restructurings and thousands of feature adjustments, VisualLogic finally achieved high satisfaction in user experience and technical framework, achieving initial market success.

Currently, VisualLogic has about 300,000 registered developers and has been used to develop nearly 1 million applications. With the introduction of AI model automatic programming, VisualLogic has made breakthroughs in the continuous evolution of underlying technology, further enhancing productivity. Over the next 30 years, VisualLogic will continue to strive to serve more users and create greater value.
 # Working Mechanism and Principles

### # VisualLogic's Working Mechanism

The core design philosophy of VisualLogic is to use the paradigm of "component-oriented programming" to break through the limitations of traditional programming languages, providing users with a graphical and visual programming environment. This design not only lowers the learning curve but also greatly enhances development efficiency, while also considering the flexibility and reusability of code.

#### # Core Ideas and Concepts

The essence of VisualLogic is a complete graphical logic editing solution. It achieves rapid expression of application logic through the provision of rich, customizable, and quickly importable components, combined with an event panel and a data-flow-based node+line panel. This expression method is Turing complete, making the development process both efficient and easy to maintain. VisualLogic initially appeared in WebApp development, and has evolved to incorporate popular technologies for both front and back ends of the Web. The frontend uses React for implementation, and the code generation is also based on React core. The middle platform uses Node.js to parse JS code, while the backend connects with cloud infrastructure through the Go language to achieve efficient and stable communication.

#### # Atomic Component Construction

In VisualLogic, applications are broken down into basic components for the abstraction and construction of general-purpose applications. Atomic components are not only foundational for the completion of application development but also form more complex models and modules. This process involves object-oriented encapsulation of components, organically combining "object properties," "trigger conditions," and "object functions."

VisualLogic employs many key principles to ensure the completeness of the language, such as the design of the event panel and data flow components. The former breaks the boundaries of code expression through condition-triggered logic design; the latter uses a node+line method to build a directed acyclic graph, thus enhancing the parallel computing capability of data flow.

#### # Event Panel and Logic Expression

VisualLogic introduces the "event panel," a condition-triggered approach that allows developers to build complex logic without coding. The event panel enables users to assemble logic through adding tree-structured logic blocks, including conditions, loops, and actions.

This trigger-based design method makes the expression of any application possible. The logical expression capability of the event panel is Turing complete, allowing developers to orchestrate logic and events together to achieve basic logic in code.

#### # Compiler Supporting Multiple Target Systems

VisualLogic not only focuses on the visual expression of logic but also provides support for various target systems. Through a compiler, it compiles component-based designs into code that can be used directly on different platforms, such as iOS, Android, Windows, Mac, and Web applications. This process involves parsing the native development languages of target platforms and translating VisualLogic-generated logic into compatible code forms via the compiler.

#### # Logic Visualization and Application Generation

VisualLogic attempts a new method to achieve logic visualization through event panels, which is one of its highlights. It not only supports the automatic generation of frontend and backend code but also provides a visual logic editor, allowing even users without a programming background to quickly understand and realize complex logical relationships.

Behind this design philosophy is the division of programming tasks into reusable component logic, substantially simplifying the complexity of the development process. Through detailed encapsulation of components, VisualLogic enables developers to accomplish the integration from component invocation to system integration within the IDE, efficiently and intelligently throughout the entire process.

#### # Conclusion

VisualLogic encapsulates a large amount of code into components and utilizes visual logic orchestration tools, such as event panels and data flow panels, allowing developers to create fully functional applications in a more intuitive and efficient way. This no-code logic orchestration not only simplifies the development process but also ensures the high quality of the generated code and the convenience it brings to enterprises. Coupled with a powerful compiler system, VisualLogic can generate application code suitable for different platforms and environments, thereby truly realizing the efficient development of "component-oriented programming."
 # Introduction to the field and technology of VisualLogic
 # # Overview

VisualLogic is a versatile visual programming language designed to connect various components through "component-oriented programming" and "graphical logic orchestration" to form complete application logic. This method allows seamless generation of both front-end and back-end code and generating the specified language or framework code based on user requirements. The flexibility of VisualLogic enables both novices in the "no-code" programming space and "code-savvy" programmers to get started easily. Front-end development can generate Vue or React framework code, while the back end generates Java SpringBoot framework code.
 # # VisualLogic IDE Structure

The integrated development environment (IDE) of VisualLogic includes the following panels and bars:

1. **Component Panel**: Used to add various components.
2. **Properties Panel**: Displays and edits component properties.
3. **Object Tree Panel**: Shows and manages all objects in the application, supporting event addition.
4. **Event Panel**: Implements complex logic using "Conditional Blocks," "Action Blocks," and "Loop Blocks."
5. **Data Flow Panel**: Conducts visual orchestration of data flows.

In the application development process, the design of components is based on "object-oriented" encapsulation, providing support in three areas: trigger conditions, properties, and methods. The Event Panel can combine different blocks to achieve complex and flexible logical expressions, while the data flow components utilize a directed acyclic graph structure to facilitate data flow between different nodes.
 # # VisualLogic Components

VisualLogic boasts a rich collection of components, including:

- **Frontend Components**: Involves visual components (such as buttons, images, and videos), container components, variable components, and communication components (such as APIs, Sockets, mqtt, etc.).
- **Backend Components**: Encapsulations of cloud computing products, such as databases, MQs, Redis, middleware, scheduled services, third-party interfaces, and AI model backend components.

VisualLogic advocates the "component-oriented programming" concept, allowing individuals who do not understand code to quickly implement complete programming applications while ensuring high code quality and easy maintenance.
 # # Relevant Technologies and Product Areas

The VisualLogic platform covers technologies and product areas including:

- **Visual Logic Programming Language**: Allows users to build logic through visual means.
- **Integrated Development Environment (IDE)**: Provides comprehensive development tools and environment.
- **Low-Code/No-Code Development**: Simplifies the development process.
- **Technical Middle Platform, Business Middle Platform, Data Middle Platform**: Increases resource efficiency and reusability.
- **Cloud-Native, Cloud Computing, and DevOps**: Supports cloud application lifecycle management and continuous integration and delivery.
 # # Features and Solutions

## # Cloud-Native

Cloud-native emphasizes a complete cloud-based experience from architecture to operations without manually "lifting applications to the cloud." In VisualLogic, users become cloud-native developers, benefiting from optimizations in development, operations, and other processes.

## # Middle Platform Concept

The middle platform in VisualLogic manifests as the reuse of technical and data resources, optimizing the development process and improving the reusability of the overall architecture.

## # Microservices

VisualLogic supports users in building and managing their microservices architecture, achieving efficient implementation through visual management of different services.

## # Serverless

VisualLogic integrates Serverless features, allowing users not to focus on implementation details. All resources are elastic and allocated on-demand, reducing usage costs and complexity.

## # DevOps

Utilizing VisualLogic’s built-in DevOps features, it shortens development cycles, realizes continuous integration and delivery, and optimizes software engineering and operations processes.

```mermaid
graph TD
A[VisualLogic Platform Usage] --> B[Cloud Development]
B --> C[Application Lifecycle Management]
B --> D[Agile Development]
B --> E[DevOps]
B --> F[Microservices]
F --> G[Public Services]
F --> H[In-group Microservices]
F --> I[Microservices under Account]
```"
 # Copyright ownership of VisualLogic applications? And copyright application?
 # # VisualLogic Applications Copyright Ownership and Copyright Application

## # VisualLogic Application Copyright Ownership

The applications developed on the VisualLogic platform have their copyrights owned by the developers. The VisualLogic platform itself provides a development environment and hosting services, but the ownership of the application belongs to the creator. VisualLogic is not involved in the usage count of the application, nor does it change the pricing strategy based on user count. Whether it is 10 users or 100,000 users, applications independently developed using VisualLogic are free from restrictions on copyright and usage. Therefore, users can confidently download their applications to their own servers without worrying about platform constraints.

VisualLogic also offers technical measures to protect user copyrights, such as protecting the source code to ensure that modified code cannot be re-released as VisualLogic code, effectively preventing infringement by users of other platforms.

## # VisualLogic Applications Copyright Application

Applications developed with VisualLogic generate downloadable code during the code generation stage, such as JavaScript, and the copyright of this code belongs to the developer. Therefore, developers can apply for software copyright for this code, just like they would for manually written code.

The application process for software copyright is similar to that for ordinary code, and developers can complete the application through third-party agents to secure their intellectual property rights.

In addition, according to the related content in the knowledge base, the code generated by VisualLogic fully meets the conditions for applying for domestic software copyright and can also apply for patents, providing developers with legal means to protect their work.

## # Summary

In summary, the VisualLogic platform provides good copyright protection measures and copyright application support for application developers, allowing them to confidently use VisualLogic for application development and have the same legal protections as traditional code development. Both in terms of copyright ownership and the copyright application process, VisualLogic demonstrates its commitment to and support for developer rights. At the same time, the platform’s transparency and fairness in charging allow developers to focus more on application design and implementation without worrying about additional financial burdens.

```mermaid
graph TB
    A[Create Application] --> B[Generate JavaScript Code]
    B --> C[Copyright Application]
    C --> D[Protection and Enforcement]
    A --> E[Deploy on VisualLogic Cloud]
    E --> F[Charge Cloud Computing Fees]
    A --> G[Download to Other Servers]
    G --> H[Independently Operate Application]
```

The VisualLogic platform, through this method, combining a graphical programming language with flexible code generation and legal support, provides developers with an efficient, secure, and easy-to-maintain development environment. Through VisualLogic, developers can enjoy the convenience of no-code development while also using code to implement more complex functions when needed.

Ensuring that developers obtain the complete intellectual property rights to the applications they create while using all the convenient features provided by the platform, and enabling them to legally and freely engage in commercial activities in the market, has always been the relentless pursuit and core support of the VisualLogic platform.
 # JWT encryption

## Overview and Application of JWT Encryption

### What is JWT?

JWT, short for JSON Web Token, is an open standard based on JSON. It allows secure and reliable information transfer between users and servers through a structured information set. A JWT is composed of three parts: the Header, Payload, and Signature, connected by dots (.), e.g., xxxxx.yyyyy.zzzzz.

### Main Functions of JWT

JWT is widely used in two main scenarios:

1. **Authentication and Authorization**  
   The most common application of JWT is in authentication and authorization. After logging in, each subsequent request contains the JWT, allowing the user to access related routes, services, and resources.
   
2. **Information Exchange**  
   JWT is an ideal way to securely transmit information between parties. Since JWTs can be signed and encrypted, the system can verify the sender's identity and ensure that the information has not been tampered with during transmission.

## JWT Application in VisualLogic Environment

### Basic Usage and JWT Workflow

In VisualLogic-based development, the JWT usage process typically follows these steps:

- **Login Verification**  
  When a user logs in, they input their account and password in the frontend and send it to the backend. After verification, the backend generates a JWT, stores it in an HTTP-only cookie, and returns it with user information to the frontend.

```mermaid
flowchart TB
    subgraph Frontend
        A[Login]
        B[User Information]
    end

    subgraph Backend
        C[Verification]
        D[Generate JWT]
    end

    A -->|Send Account and Password| C
    C --> D
    D -->|Set HTTP-only Cookie and Return User Information| B
```

- **Subsequent Requests**  
  For subsequent accesses or data requests, the frontend provides the JWT from the cookie to the backend. The backend parses the JWT to obtain user information and returns data or error messages based on the decryption result.

```mermaid
flowchart TB
    subgraph Frontend
        A[Re-login or Request Data]
        D[Receive Returned Data and Display Corresponding Content]
    end

    subgraph Backend
        B{Get JWT from Cookie and Decrypt}
        C[Return Requested Data]
        E[Return Error Data]
    end

    A --> B
    B -->|Decryption Successful| C
    B -->|Decryption Failed| E
    C --> D
    E --> D
```

### JWT Generation and Management

1. **Generate JWT (Backend)**  
   To create a JWT, the system needs the original data (e.g., user ID) and an encryption key. The encryption key is generated and stored in the backend, typically with a length of 64, 128, 256, or 512 bits, used for encryption and decryption.

2. **Set Cookie (Backend)**  
   After generating the JWT, the backend sets it as a cookie, including the JWT and custom attributes like expiration time and name.

3. **Get Cookie and Decrypt JWT (Backend)**  
   The backend retrieves the cookie based on the JWT name and decrypts it using the same encryption key to obtain the original data. Thus, the encryption key is always kept in the backend, ensuring it is not leaked.

### Advantages of JWT in VisualLogic Products

- **Code-free Flexibility**  
  Utilizing the VisualLogic graphical programming environment, developers can quickly integrate and manage JWTs through components and graphical tools without writing complex code.

- **Efficient Component Structure**  
  VisualLogic's component-based development allows developers to rapidly develop JWT-related functions via simple drag-and-drop and configuration, significantly improving development efficiency.

- **Powerful Backend Encryption Capabilities**  
  VisualLogic supports the integration of various encryption components and algorithms, better protecting user information during JWT generation and decryption.

### Considerations

- Original data should include necessary information but should not contain sensitive data, such as passwords.
- The encryption key is crucial and should be kept in the backend for JWT generation and validation, not leaked to the client.
- Every service request that requires authentication should first decrypt the JWT to confirm the requester's identity.

### Attachments

- [JWT Encryption Demo Download](https://www.ivx.cn/ih5/app/template/download?id=3596)"
 # VisualLogic VS Programming language

##### Main Differences

VisualLogic, as an innovative graphical programming language, has significant differences from traditional programming languages in several aspects. These differences not only manifest in development methods and technical details but also fundamentally change the developers' way of thinking and workflow.

##### Graphical vs. Textual

VisualLogic uses a graphical approach to express application logic and processes, making it more intuitive and understandable, especially suitable for beginners without a programming background. In contrast, traditional programming languages rely on text code, requiring developers to have a solid programming foundation and a deep understanding of syntax and code structure.

##### Decoupling of Logic and Syntax

VisualLogic greatly simplifies programming complexity by decoupling "logic expression" from "syntactic features." When using VisualLogic, developers can focus more on the logic itself without being overly concerned with syntactic details. In traditional programming languages, logic and syntax are closely connected, increasing the difficulty of learning and mastering them.

##### Component-Based Programming

VisualLogic encapsulates commonly used functions and libraries into visual components, allowing developers to quickly implement specific functions through drag-and-drop and simple configuration. This is significantly different from traditional programming languages, where functions need to be manually written and called, requiring an in-depth understanding of these functions.

##### Code Generation and Compilation

VisualLogic does not rely on traditional compilers but uses a code generator to convert application logic into code in specified languages, such as Vue, React, or SpringBoot. This process is simpler for developers, while traditional languages require a complex compilation process to convert source code into executable machine code or bytecode.

##### Catering to Different Developer Groups

VisualLogic is designed for full-stack developers, enhancing the unification of application development and code generation, and is suitable for various development scenarios. Traditional programming languages usually focus more on enterprise services, requiring specific runtime environments and system support.

##### Flexibility and Ease of Use

The original design of VisualLogic emphasizes simplicity and rapid implementation, even if it sacrifices flexibility to some extent. Traditional programming languages, on the other hand, emphasize flexibility and comprehensive functionality, often leading to increased complexity in use.

#### Advantages of VisualLogic

By analyzing the differences in design concepts and implementation between VisualLogic and traditional programming languages, we can summarize the advantages of VisualLogic in the following aspects:

##### Learning, Development and Operations Efficiency

The learning curve of VisualLogic is gentler compared to traditional languages. For those with a programming background, VisualLogic can be mastered to become a full-stack developer within one to two weeks. Traditional programming languages, on the other hand, often require several years to fully master front-end, back-end, and operations technologies. Meanwhile, due to its graphical and component-based design, VisualLogic significantly improves development efficiency, reduces the occurrence of bugs, and provides efficient operational support.

##### Code Quality and Maintainability

Through visual operations, VisualLogic effectively avoids code spelling mistakes and maintains a clear logical structure, making the code easier to understand and maintain. In contrast, hand-written code in traditional programming languages is more prone to include spelling and logic errors, increasing the complexity of maintenance.

Overall, VisualLogic not only lowers the barrier to programming, allowing developers to complete application development more efficiently, but also solves the maintenance challenges posed by the complexity of traditional programming languages by generating high-quality code. These characteristics make VisualLogic an emerging technology of widespread interest in the future software development field.
 # Methods for connecting to third-party databases

## Overview

VisualLogic allows users to connect and operate third-party databases, providing great flexibility and convenience for application development. Common connection methods include API connections and DBO mode connections. In API connections, VisualLogic performs database operations via interfaces and services provided by third parties. For third-party databases that are not deeply integrated, such as Oracle, SQL Server, PostgreSQL, etc., connections and access are realized through DBO mode.

## Use Cases

- **API Connection**: Suitable for scenarios utilizing third-party database services, performing data retrieval, storage, and management via API calls.
- **DBO Mode**: Suitable for developers needing to manually write SQL statements to access databases, providing flexibility for manual control and database access.

## Core Features

### API Connection

VisualLogic can encapsulate various types of services (API interfaces) for access by external systems. The types of services provided include:

- **General Services**: For access within the application.
- **Microservices**: Divided into enterprise microservices and intra-group microservices.
- **Public Services**: Services accessible via the internet (IP address).

### DBO Mode

- For third-party databases (such as Oracle, SQL Server, PostgreSQL), control and access need to be performed through VisualLogic's DBO object layer.
- Developers are required to manually write SQL statements for the target database, which VisualLogic will directly send as needed to complete database access.

### VisualLogic Built-In Database

- Supports generating corresponding SQL statements directly through the event panel, covering the following databases:
  - MySQL
  - ElasticSearch
  - TableStorage (AWS DynamoDB)
  - ObjectStorage (AWS S3)
  - Redis
- Supports Abstract Syntax Tree (AST), automatically converting the structure and target database SQL equivalent syntax.

## Basic Usage

### API Connection Examples

1. **Set Up API Interface**: Configure the API interface of the third-party database on the VisualLogic platform.
2. **Invoke API**: Retrieve or operate on database data via event triggers or direct calls.
3. **Process Return Data**: Handle and display the returned database data as required.

### DBO Mode Examples

1. **Write SQL Statements**: Manually write SQL statements for the target database, ensuring correct syntax.
2. **Configure DBO Object**: Create and configure DBO objects in VisualLogic to send SQL statements to the database through the application layer.
3. **Execute Query**: Perform SQL queries in the application through event triggers or calls and retrieve results.

## Extraction Notes

- Ensure the security and correctness of the interface when connecting via API, and verify that the returned data format meets requirements.
- Ensure the security of SQL statements when using DBO mode to prevent SQL injection vulnerabilities.

## Further Reading

For more complex implementation needs, refer to VisualLogic's official documentation and sample projects provided by the related community. This will help developers better understand how to flexibly apply these features to meet specific business requirements.
 # What is a micro-module?

### **Feature Overview**

Mini-modules are user-defined "functional combination units." Once created, these mini-modules can be added to the object tree for use just like regular components. This feature allows developers to modularize custom functionalities, thus enhancing development efficiency. Additionally, users can upload completed mini-modules to a cloud-based library for sharing across different applications.

### **Role of Mini-modules**

Mini-modules serve two primary purposes:

1. **Function Reusability**: Frequently-used functionalities can be packaged and uploaded to avoid redundant work. For example, if a custom-styled popup is required across multiple applications, it can be made into a mini-module. When needed, it can simply be added. This method is more efficient than copying or cross-case copying. Especially during version updates, the importance of using mini-modules becomes evident. Modifying a mini-module and updating its definition synchronizes all instances of that module.
   
2. **Function Module Packaging**: Even when certain function modules do not require reuse, they can still be packaged as mini-modules. This encapsulates complex object tree structures, organizing complex functionalities structurally. This ensures independence between internal and external functionalities, enhancing application comprehensibility and maintainability.

### **Types of Mini-modules**

Mini-modules are categorized into three types: Frontend Mini-modules, Backend Mini-modules, and Composite Mini-modules:

- Frontend Mini-modules: Can only be added under the frontend root.
- Backend Mini-modules: Can only be added under the backend root.
- Composite Mini-modules: Upon addition, respective instances are added under both frontend and backend roots.

### **Mini-module Definitions and Instances**

It is crucial to distinguish between "Mini-module Definition" and "Mini-module Instance."

As illustrated, the creator of a mini-module is responsible for making the mini-module definition. When a mini-module is added to an application, corresponding mini-module instances are generated based on the definition. If you consider a mini-module as a "component" of a product, then the mini-module definition is the "master template" of this component, and mini-module instances are the actual components. A single master template can produce multiple component entities.

In VisualLogic, mini-module definitions are stored as part of the application's main structure in a dedicated mini-module root. All mini-module instances are produced based on these definitions.

In the application illustrated, two mini-module definitions were added, with instances created under two frontend pages. Note that the same mini-module definition can create multiple instances on different pages.

### **In-app vs. Cloud Mini-modules**

VisualLogic offers two modes for creating mini-modules: In-app Mini-modules and Cloud Mini-modules.

1. **In-app Mini-modules**: Created within the application and usable only within that application.
2. **Cloud Mini-modules**: Stored in the cloud, available for download by different applications and supporting version-synchronized updates.

In-app and cloud mini-modules have different creation entry points. Once created, each type follows a different usage process.

In the shown example, in-app mini-modules are directly added to the in-app mini-module definition pool, whereas cloud mini-modules open a dedicated editing shell application. After editing, they can be published and uploaded to the cloud module library. When an application requires a cloud mini-module, it must be downloaded to the local mini-module definition pool. If a cloud mini-module undergoes a version update, it can be updated in the in-app mini-module definition pool.

Regardless of whether it is an in-app or cloud mini-module, it first needs to be added to the in-app mini-module definition pool before adding corresponding mini-module instances to the object tree.

### **Conclusion**

As part of VisualLogic, mini-modules significantly enhance development efficiency and application maintainability. Through function reuse and module packaging, developers can manage complex application logic more conveniently, avoid repetitive work, and ensure independence and inter-operability among different parts. By distinguishing mini-module definitions and instances, as well as in-app and cloud modes, mini-modules offer flexible and robust functionalities, achieving modular and component-based development experiences.

```mermaid
graph TD
    Create Mini-module --> Add to Mini-module Definition Pool
    Add to Mini-module Definition Pool --> Mini-module Instance Addition to Object Tree
    Cloud Mini-module --> Open Editing Shell Application
    Open Editing Shell Application --> Publish to Cloud Mini-module Library
    Download from Cloud Mini-module Library --> Add to Local Definition Pool
    Local Definition Pool Update --> Instance Update in Object Tree
```"
 # What is a custom component?
 # # Feature Overview

Custom components are a feature provided by VisualLogic, allowing developers to add frontend components to VisualLogic projects independently. Developers can create and add custom components based on the project requirements, and these custom components can be used just like the system's built-in components within the project.

Since the current frontend projects in VisualLogic are developed based on the React framework (version 16.13), all custom components must be implemented following the React component development methodology. Developers need to have a certain foundation in using the React framework to develop custom components.
 # # Custom Component Development and Usage Process

The development and usage of custom components involve the following steps:

1. **Component Development**: The component developer needs to complete the component development first to ensure the component functions as expected.
2. **Component Library Upload**: Upload the defined and developed component library to the cloud library. A library can contain multiple components or just a single component.
3. **Component Library Download**: Application developers download the component library definition from the cloud library to the application's component library. It is important to note that the smallest storage unit for custom components is the "library", and components can only be downloaded as a library, not individually from the component library.
4. **Component Instance Creation**: After downloading the component library definition into the application, developers can add component instances to the project and use these custom components just like system components.
5. **Component Definition Update**: When there is an update in the cloud component library version, developers can update the component library definition within the application. Once the definition is updated, all component instances from the library used in the application project will also be updated synchronously.
 # # Features and Advantages

1. **Enhanced Extensibility**: The custom component functionality significantly enhances the extensibility and flexibility of VisualLogic projects. Developers can create bespoke components to meet specific project requirements.
2. **Unified Management**: Component libraries are managed and updated as a whole, ensuring consistency and uniformity among components in the project. Following a version update, all component instances referencing the library will automatically update synchronously, reducing the hassle of manual updates.
3. **Efficient Development**: Uploading developed components to the cloud library for reuse in other projects saves development time and resources, improving work efficiency.
4. **User-Friendly Component Usage**: Through drag-and-drop and visual operations, developers can flexibly use custom components in projects without writing complex code, making it very suitable for users unfamiliar with programming.
 # # Considerations

1. **React Foundation**: Since custom components are developed based on the React framework, developers need to have a certain foundation in React.
2. **Component Library Management**: The component library downloaded into the application is only a definition of the component; the actual use in projects involves component instances. When the cloud component library is updated, project component instances will automatically synchronize updates, but it's still essential to ensure good management of the component library.
3. **Version Control**: Multi-version management of the component library is crucial for project maintenance. Developers need to pay attention and update component library versions timely to ensure the components in projects are in their latest and optimal state.

Through the brief introduction and detailed explanation of the development process of custom components, it can be seen that VisualLogic provides an efficient, flexible, and easy-to-manage component management model for both developers and users through its custom component functionality. This not only improves development efficiency but also enhances the application development experience.
 # Component Development Environment

## # Creating a Custom Component Library

**Under the components panel - custom section, we can create a new custom component library:**

After clicking the create button, the system will establish a "shell application" for the custom component library. This shell application is similar to a regular application and will appear in the workspace, but it cannot be published or listed; it is used only for editing and debugging custom components.

## # Component Editing Mode and Preview Mode

The shell application editor for custom components offers two editing modes:

1. **Component Library Development Mode**: Used for coding custom components.
2. **Component Library Preview Mode**: Provides a regular application editing environment, allowing you to add newly developed components for preview like a standard application.

**The editing interface in Component Library Development Mode is as follows:**

- The left object tree section is used to add constituent parts of the component library. The method of adding and using these parts is similar to components in regular applications. Here, the sub-objects added under the object tree are referred to as "parts."
- The right editing panel resembles the property panel of regular objects, used for writing component code.

**The interface in Component Library Preview Mode is as follows:**

The preview mode interface is identical to the regular application editing interface, with the only difference being the "In-Development Component Panel" located at the bottom right. This panel can add various components contained within the current shell application for functionality preview. Additionally, other VisualLogic components can be added for preview. For instance, a regular button can be added with a click event to trigger the appearance of a pop-up custom component.

**Note**, custom components under development must be compiled before they appear in the "In-Development Component Panel."

```mermaid
graph TD;
    A[Creating Custom Component Library] --> B[Component Library Development Mode]
    A --> C[Component Library Preview Mode]
    B --> D[Add parts to Left Object Tree]
    B --> E[Write Code in Right Editing Panel]
    C --> F[Add Components from Shell Application]
    C --> G[Add VisualLogic Components for Preview]
    G --> H[Add Button]
    H --> I[Trigger Pop-up Custom Component]
```"
 # Component Development Workflow

## # Composition of the Component Library

Each component library is composed of the following parts:

## # Steps to Develop a Component Library

In the following sections, we will detail how to develop a custom component from scratch.

Firstly, we provide three shell application templates for custom components. In these three templates, we introduce three commonly used UI libraries: Ant Design, Element, and Material. Each library has four demo components created, including a button, a modal dialog, a dropdown menu, and a data table.

The main steps to develop a custom component library include:

1. Import NPM packages (optional)
2. Add components and write component JSX files (core steps, mandatory)
3. Fill in property, method (action), and event configuration documents (optional based on component requirements)
4. Compile and debug
5. Upload and publish the component library

Next, we will take Ant Design's component library as an example to explain each step in detail.

## # Import NPM Packages

Custom components support importing one or more external NPM packages, or they can be developed without any NPM package.

In our example using the Ant Design component library, we need to import two NPM packages:

We do not specify the version for now; the system will automatically use the latest version of the package during component compilation.

## # Start Writing Component JSX

The component JSX file is the core code definition of the component and cannot be empty. Next, we will continue to use the Ant Design library as an example to explain the key points of editing a JSX file.

1. First, import the related libraries needed by this JSX:

```jsx
import React from 'react'
import { Button } from 'antd'
import 'antd/dist/antd.css'
```

2. Start writing the class:

```jsx
export default class XXXX extends React.Component {
    // Write code here
}
```

Note that the class name XXXX must start with an uppercase letter.

For example, the JSX code for a button component is shown below:

### Develop Component Properties

The creation of component properties includes two parts:

1. Define property rendering rules in the JSX file:

In the render function, we first define the internal property collection of the VisualLogic component. The properties in this collection are those we want to appear in the property panel.

First, we correlate the properties built into the component's library with the property names in VisualLogic. In this example, we did not make any modifications and used the original names of the library components. Later, we can further translate and explain these properties in the property configuration document.

Next, we added an additional property called visible, which is the "visibility property" in the VisualLogic editor. When this property is turned off, the component is not rendered, which allows for easy control in the editor.

We can set default values for our properties; this step is optional:

2. Configure Property Documentation

The property configuration document of a component is another component construction part used to define the behavior of the property panel:

Its internal structure is a JSON-formatted file:

The JSON structure is a standard object array, representing multiple properties. Each array element is an object. Aside from the name and type, other fields are optional. The significance of each field is explained below:

### Define Component Methods

The definition of component methods also includes two parts:

1. Write method code in the JSX file:

In the JSX class definition, we need to declare the corresponding methods:

For instance, in the above code snippet, we defined two methods for a modal dialog component.

2. Fill in the Method Configuration Document

The method definition JSON file is also an object array. For example, the configuration document for the methods defined in the JSX of the modal dialog component above is written as follows:

All the field descriptions inside the method objects are as follows:

Among these, params is an array used to define the method's input parameters, with each parameter described within the array's objects:

### Define Component Events

The definition of component events also includes two parts:

1. Define events in the JSX file:

First, in the render function, we need to bind the methods we will use to a property class:

As shown in the image above, in the button component, the first onClick is a method property that we bind to a method onBtnClick() of the current custom component.

The definition of this onBtnClick is as follows:

In this, "this.props.onClick" is a check to determine whether the "click" event has been passed in from the outside, and if so, the subsequent event trigger method will be executed. Note that the first letter of the event name following "on" needs to be uppercase. For example, if the event name defined in the event configuration document is "click," the method here should be written as "onClick."

If you need to handle event parameters and expose them to the user, you need to specify parameters when defining the method:

For example, in the "Select Option" method definition of the dropdown menu component above, we pass in a parameter "value."

2. Configure Event Documentation:

The event configuration document of a component is used to define the trigger events in the event panel.

Its internal structure is also a JSON file:

In the event configuration document above, we defined two events, one for initialization and one for clicking. The clicking event will further throw out the event parameters, which can be referenced in the event logic when the event is triggered.

Each event definition's field descriptions are as follows:

The params field descriptions are as follows:

In event definitions, the names in the configuration document have the following correspondence with method definitions in the JSX file:

The red part is the event name. Note that in the method definition in the JSX file, the event name's first letter needs to be capitalized, and "on" should be added in front to specify it's an event handler. The purple part is the parameter name, and once parameters are defined, users can refer to them when the event is triggered.

### Methods for Component Communication

During usage, components often need to interact and communicate with external components. Here, we take modifying the component's own visibility attribute as an example to introduce some basic methods for component communication:

First, we set the visible attribute for the component:

Then, in the render function, use the visible attribute to determine whether to render the component:

Effect:

#### Method 1: External "Set Property"

"Set Property" allows updating the component properties, enabling external components to communicate with internal components. The diagram below shows how the component's "visibility" can be modified:

#### Method 2: Internal Event Throwing

Sometimes, components need to send data to the outside, which can be achieved by triggering a component event to send data out. The following demonstrates how a component uses event throwing to set its own visibility attribute:

First, define a component event named changeVisibleEvent, and the corresponding trigger function for this event is props.onChangeVisibleEvent.

Then, the component internally triggers this event and attaches a visible value:

Throwing the event does not modify the visible value; here, we also need to capture the event and assign the value accordingly:

#### Method 3: vModel Data Binding

If you want to start from a function inside the custom component and modify its own properties, you can use the vModel method to bind the properties.

First, set the vModel value for the component:

As shown above, vModel is a string array, with each element split into three parameters by a comma: each array element represents a bound property field:

- Parameter 1: The method name for triggering the property update event. It's like registering an event, where parameter one is the event's name that can be triggered by calling the event method.
- Parameter 2: The name of the bound property, i.e., the property field name that needs to be updated.
- Parameter 3: The name of the parameter passed in.

After vModel is set, the component can actively modify its (parameter two) property inside the component using the following method:

```jsx
this.props.onEventNameWithCapitalLetter({
parameter3: xxx
})
```

Referring to the method two button click to trigger the vModel method, you can achieve the same effect of props change. The final effect is the same as the event throwing method, but it skips the event capture process.
 # Debugging and Deployment of Components

## Debugging Components

To debug components in VisualLogic, follow these steps:

1. **Click Compile Component**: First, click the compile component button and wait for the compilation to finish.
2. **Switch to Component Library Preview**: After compilation is complete, switch to the component library preview.
3. **Add Test Component**: Add the component you want to test to the case from the "Custom Component Library" below the object tree.
4. **Modify Properties and Events**: After adding the component, you can modify its properties or add event actions, etc. Once finished, you can preview the component library to see the actual effect.

The flowchart for the above steps is as follows:

```mermaid
graph TD
    A[Click Compile Component] --> B[Wait for Compilation to Finish]
    B --> C[Switch to Component Library Preview]
    C --> D[Add Component to Be Tested from Object Tree to Case]
    D --> E[Modify Properties or Add Events]
    E --> F[Preview Component Library to See Actual Effect]
```

## Publishing, Uploading, and Usage

To publish and upload the custom component library you’ve created and use it in other cases, follow these steps:

### Uploading the Created Custom Component Library

1. **Upload Component Library**:
   - In the "Component Library Edit" interface, click publish and upload.
   - Enter the name, description, icon, and other information.
   - Click complete to finish uploading the custom component library.

### Using the Custom Component Library in Other Cases

1. **Use Custom Category**:
   - Click the custom category in the left component bar.
2. **Find and Download Custom Component Library**:
   - Find the custom component library to be downloaded in the cloud component library.
   - Clicking the custom component library will automatically download it.
3. **Select and Use Downloaded Component Library**:
   - Select the corresponding downloaded library, choose the components within it, and after clicking, the selected components will appear in the object tree, thus successfully using the custom components.

The flowchart for the above steps is as follows:

```mermaid
graph TD
    A[Upload Created Custom Component Library] --> B[In Component Library Edit Interface Click Publish Upload]
    B --> C[Enter Name Description Icon etc.]
    C --> D[Upload Successful]
    D --> E[Click Custom Category in Left Component Bar]
    E --> F[Find Custom Component Library in Cloud Component Library]
    F --> G[Click Custom Component Library to Auto Download]
    G --> H[Select Components in Downloaded Library]
    H --> I[Selected Components Appear in Object Tree]
    I --> J[Custom Component Successfully Used]
```

## Summary

By following the above steps, we can effectively debug, publish, and use components in the VisualLogic environment. These operations not only simplify the development process but also make the management and application of the entire component library more efficient. Whether you are a beginner or a professional developer, you can easily get started and efficiently leverage VisualLogic for project development.
 # Standalone Deployment 1: Launch VisualLogic Program

### Standalone Deployment of VisualLogic Program

#### 1. Install MySQL and Redis

VisualLogic applications require two underlying databases: MySQL and Redis. We recommend using cloud-hosted MySQL and Redis services, such as Alibaba Cloud RDS and Cloud Database Redis Edition. For the sake of convenience and deployment cost, this demonstration will install MySQL and Redis on a local server.

**Minimum version requirements:**

- MySQL: 5.7.9
- Redis: 2.8

On a CentOS system, you can use the `yum` command to install these software packages. For detailed steps, please refer to the following links:

- [Install MySQL on CentOS7](https://www.yuque.com/docs/share/c7c348a9-6d88-4924-8f60-bc2ae49b8eb3)
- [Install Redis on CENTOS7](https://www.yuque.com/docs/share/d850a2ef-689d-4203-bede-749ba4275316)

#### 2. Create Directory and Upload Program Files

**Step 1: Connect to the Server**

It's recommended to use SSH to log into a Linux instance. Here is the connection command:

```shell
ssh <username>@<hostname or IP address>
```

Enter the password to complete the login. After a successful login, you can use the `ls` command to view files in the current path.

**Step 2: Create a New Directory**

Create a new directory, for example, `testA`:

```shell
mkdir testA
```

**Step 3: Download and Upload Base and Application Files**

Download the base and an application file, ensuring that their paths contain no Chinese characters. Use the `scp` command to upload files to the newly created `testA` directory:

```shell
scp <filepath> <username>@<IP address>:<path/>
```

Switch to the `testA` directory and check that the files have been successfully uploaded:

```shell
cd testA/
ls
```

**Step 4: Make ivxbase Executable**

Execute the following command to make ivxbase executable:

```shell
chmod +x ivxbase
```

#### 3. Initialize ivxbase

**Step 1: Run ivxbase**

Enter the following command in the current directory to run ivxbase:

```shell
./ivxbase
```

Fill in the configuration items as prompted. Make sure not to occupy port 80; you may use another custom port such as 9966.

**Step 2: Fill in Redis and MySQL Configuration**

Use the locally installed Redis and MySQL:

- Enter the Redis address and user, all set to default options.
- Enter the MySQL address, user, and password.

After running, the command line will print out the application path, indicating a successful deployment. The access format for the case is `IP+port+path`.

The following example refers to using an Alibaba Cloud RDS database. If using local MySQL, set the defaults.

You can stop the current process by using cntrl+c to stop the case from running.

#### 4. Detailed Steps of Program Deployment

**Step 1: Upload and Run Java Package File**

1. Export the backend source code (Java) - a complete Java project.
2. Open the unzipped IvxJavaService folder with Visual Studio Code locally.
3. Execute `gradle run` in VS Code, then terminate by pressing `ctrl+c`; continue with `gradle distZip` to generate JavaExtServer.zip.
4. Upload the generated IvxJavaService.zip package to the server running the VisualLogic application and unzip it.
5. In the unzipped IvxJavaService/bin directory, execute `./IvxJavaService`.
6. To add multiple applications in this JAVA project, choose "only application code files," paste the exported `ServiceXXXXX.java` into the `src/main/java` directory, then execute `gradle genMain` and `gradle distZip` in the project's root directory.
7. Upload the newly generated JavaExtServer.zip to the cloud server, unzip it, and enter the bin directory to execute `./IvxJavaService` to run multiple cases.

**Step 2: Download and Upload Application Export Package**

The application export package contains front-end code and some basic service configuration information.

**Step 3: Run the VisualLogic Base Interface Library**

Add `"mainService": "localhost:8800"` in the config.json, with a default port of 8800. If the default port needs to be changed, it can be adjusted in `AppServer.java`.

### Summary

Following the above steps, you can successfully deploy VisualLogic in standalone mode. This deployment method is flexible and efficient, allowing both non-coding learners and seasoned programmers to quickly start development and deployment. In practical applications, by configuring and managing reasonably, you can fully leverage the advantages of VisualLogic to streamline development processes and enhance development efficiency.
 # External File System (Alibaba Cloud NAS)

#### What is an External File System

An external file system can be imagined as an externally connected network drive on your computer. In Alibaba Cloud, it is called "Alibaba Cloud NAS"; whereas on Amazon Cloud, it's called "Amazon Elastic File System", abbreviated as EFS. This document takes Alibaba Cloud as an example: Alibaba Cloud File Storage NAS is a shared-access, elastic-scalable, highly reliable, and high-performance distributed file system that supports thousands of elastic cloud ECS instances sharing files within the same "Alibaba Cloud NAS".

#### Basic Usage

1. **Activate NAS File System**

   - [Activate NAS File System](https://nasnext.console.aliyun.com/introduction)
   - After successful activation, enter the "File Storage NAS" console, switch to the physical region where the ECS is located in the top left corner (e.g., North China 5 as shown below), and click to create a General-Purpose NAS File System.

2. **Create General-Purpose NAS File System**

   - In the pop-up configuration window, configure as shown in the figure below. Note that the "VPC" should be selected as the VPC in ECS (here the default VPC is selected).
   - After clicking "Purchase Now", a new file system will be automatically created. Click on the name of the file system to enter.

3. **Mount and Use**

   - After entering, click on Mount and Use, and copy the command within the box for later use.
   - Click "Mount File System to ECS", and enter into the CentOS command line the commands marked 1, 2, 3, and 4 as shown in the figure below. Note that the path in point 3 needs to be replaced with a folder path (this folder will automatically transfer data to Alibaba Cloud NAS in the future).

4. **Connect ECS Host and Enter Commands Sequentially**

   - After connecting to the ECS host, enter commands in sequence to succeed.
   - In the future, you can upload files to `/home/tande/aliyun_nas` as shown above, and they will be naturally uploaded to the "Alibaba Cloud NAS", a massive network drive with a capacity of 10PB.

```mermaid
graph TD
A[Activate NAS File System] --> B[Enter File Storage NAS Console]
B --> C[Create General-Purpose NAS File System]
C --> D[Mount File System to ECS]
D --> E[Connect ECS Host and Enter Commands Sequentially]
E --> F[Automatically Upload Files to Alibaba Cloud NAS]
```

#### Scheduled Deletion of "Alibaba Cloud NAS" Files

Scheduled deletion of files consists of two parts: deletion and scheduling.

1. **Deletion**

   - Use the `find` command to locate folders in the corresponding absolute path and delete all files within those folders.
   - Example command: `find /home/tande/aliyun_nas/sample_file -type f -name "*" -exec rm -rf {} \;`
   - The orange part is the path of the folder whose files need to be emptied.

2. **Scheduled Execution**

   - Use the `crontab` command to execute specific commands periodically.
   - First, open the vi text editor using the command `crontab -e` and write the deletion commands that need to be executed at regular intervals.
   - After entering the vi editor on Linux, click the `i` key (insert) on the keyboard and enter the following command:
     `* * * * * find /home/tande/aliyun_nas/sample_file -type f -name "*" -exec rm -rf {} \;`
   - After completing the input, press the ESC key and then enter `:wq` to save and exit the vi editor, and the scheduled service will take effect automatically.
   - The five asterisks at the beginning of the command correspond to the time interval; filling all with asterisks means executing every minute; 1440 * * * * means executing every 1440 minutes (i.e., once a day). For more detailed instructions, refer to crontab usage.

3. **Delete Scheduled Tasks**

   - If you need to delete scheduled tasks, simply enter `crontab -r` to remove the scheduled task.

```mermaid
graph TD
A[Scheduled Deletion Consists of Two Parts] --> B[Deletion]
B --> C[Delete Specified Files with find Command]
A --> D[Scheduled Execution]
D --> E[Implement Scheduled Tasks with crontab]
E --> F[Set crontab Scheduled Command]
F --> G[Delete Scheduled Tasks with crontab -r]
```"
 # Configuration file settings
 # # 1. Comprehensive Overview of the config File

The config.json file serves as the configuration file for the operation of the VisualLogicBase program. Upon the first execution of VisualLogicBase, an auto-completion feature is available on the command line, though further modifications can be made after the initial run. Note that each time the config file is modified, a restart of VisualLogicBase is mandatory for the changes to take effect.

For clusters, we recommend using a shared file system to store the config file, eliminating the need to manage multiple config files. In container deployments, the content of the config file must be included in the container cluster configurations (refer to container deployment). The container orchestration system will automatically configure the VisualLogic information into each docker instance without additional processing.

Regardless of single-node/cluster or container deployment, the content of the config file remains consistent and includes the following sections:

* nids: Configures the case nids for which the current VisualLogicBase instance is responsible.
* port: Server port;
* https: Enforce HTTPS by default set to false;
* secret: Secret key configurations
* redis: Redis address and password
* database: MySQL database configuration
* oss: File storage and CDN addresses
* sms: SMS resource address for user and SMS component message-sending functions
* email: Email service address
* loadPackage: Application package update settings
* limit: Concurrency restrictions
* Environment variables
 # # 2. Detailed Configuration Instructions

## # nids

The case IDs that the current VisualLogicBase needs to operate, represented as a one-dimensional array, for example:

```
"nids": [123456,234567,345678]
```

When adding or removing a case, this configuration item must be manually edited; otherwise, a new package will not take effect.

## # redis

Redis address and password, for instance:

```
 "redis": {
    "addr": "127.0.0.1:6379",
    "passwd": "",
    "db":0
}
```

The password can be empty. Note that for cloud-based Redis, a whitelist is often required for normal access.

## # database

MySQL database configuration, corresponding to the private deployment database in the editor's database component, for example:

```
"database": {
    "driver": "mysql",
    "addr": "127.0.0.1:3306",
    "user": "root",
    "passwd": "vxplo",
    "dbName": "h5work"
  }
```

Driver, currently supporting MySQL only. Note, the driver here refers to the database corresponding to the database component, unaffected by the dbo component.

dbName is the database name where the VisualLogic application resides on the target database instance. It can be specified at will; if it doesn’t exist, one will be created automatically during the initial VisualLogicBase setup. Once set, it’s recommended not to change it, as it might cause different backend databases containing tables of successively deployed applications, leading to access issues.

## # OSS

Configuration for object storage: Mainly used for integrating Alibaba Cloud, Tencent Cloud, AWS, and Microsoft Azure's object storage services, with the server’s local storage used by default. Below is the configuration for Tencent Cloud object storage:

```
"oss": {
    "pathPrefix": "//project-test-1304171121.cos.ap-nanjing.myqcloud.com/project",
    "provider": "tencent",
    "endpoint": "cos.ap-nanjing.myqcloud.com",
    "accessKeyId": "AKID******************JoCHD",
    "accessKeySecret": "qlY73*************MmvqUxF",
    "bucket": "project-test-1304171121"
}
```

* "pathPrefix": File path prefix (working folder path), consists of the access domain name of the object storage and a custom working folder set.
* "provider": Service provider, according to the cloud service provider, fill in:
  * Local file system: filesystem
  * Alibaba Cloud: aliyun
  * Tencent Cloud: tencent
  * Amazon: aws
  * Microsoft Azure: azblob
  * Minio: minio
* "endpoint": Regional endpoint, empty for local. Required for object storage, consisting of the object storage access domain name minus the bucket name part.
* "accessKeyId" and "accessKeySecret": API keys for operating object storage, empty for local.
* "bucket": The bucket name.

## # loadPackage

Application package update settings:

```
"loadPackage": {
    "type": "oss",
    "dir": "zip",
    "period": 1,
    "restart": true
}
```

* "type": File source: file (local), oss (using object storage)
* "dir": Path relative to the working folder path, specifies the location to check for application package updates. For type file, dir defaults to "./", indicating the current file directory (in the same directory as VisualLogicBase), while for oss, the dir set path is relative to the application’s work folder path.
* "period": Auto-check application package update interval, in seconds. If using object storage, setting this too low may incur network costs due to frequent requests, so a larger number is recommended, only reducing it when updating application packages.

## # sms

SMS configures the SMS field address, primarily for verification services within user components and services within the SMS component.

```
"sms": {
    "provider": "aliyun",
    "accessKeyId": "obtained after registering an account on Alibaba Cloud",
    "accessKeySecret": "obtained after registering an account on Alibaba Cloud",
    "templateConfig": "Registration verification: VisualLogic official, SMS_8490676; Identity verification: VisualLogic official, SMS_8490680; Change verification: VisualLogic official, SMS_8490673; Queue service notification: critical event, SMS_160571343; Service change notification: critical event, SMS_160571341; Fee depletion notification: critical event, SMS_160576413; Service start notification: critical event, SMS_160576418; Service completion notification: critical event, SMS_160760330; Service expiration notification: critical event, SMS_160576409; User identity verification: critical event, SMS_8490680; Expired service notification: critical event, SMS_160765253"
  }
```

When applying for an SMS template, the user component sends a fixed inbound parameter message; the code parameter must be submitted when applying for the SMS template (other text content can be modified freely). Specific templates are as follows:

* Registration verification: Verification code ${code}, you are registering as a new user, thank you for your support!
* Identity verification: Verification code ${code}, you are verifying identity, do not share it with anyone!
* Change verification: Verification code ${code}, you are attempting to change important information, please keep your account information secure.
* Queue service notification: Your service ${arg1}, there are ${arg2} ahead of you, please stay tuned.
* Service change notification: Your service ${arg1}, ${arg2} has changed to ${arg3}, please stay tuned.
* Fee depletion notification: Your service ${arg1}, balance is less than ${arg2} yuan, please recharge promptly.
* Service start notification: Your service ${arg1}, has started, hereby notified.
* Service completion notification: Your service ${arg1}, has been completed, hereby notified.
* Service expiration notification: Your service ${arg1}, will expire on ${arg2}, please handle renewal promptly if needed.
* Confirm user identity: Verification code ${code}, you are undergoing ${product} identity verification, do not share it with anyone!
* Expired service notification: Your service ${arg1}, has expired, hereby notified.

## # limit

Concurrency restriction settings:

```
 "limit": {
    "index": 5,
    "api": 2,
    "vmTimeout": "30s",
    "dbTimeout": "15s"
}
```

* "index": Represents the permissible number of visits per minute, 0 means no restriction. If the limit is reached, the page will indicate that the application is experiencing high traffic.
* "api": Represents the permissible number of requests per second, 0 means no restriction. If the limit is reached, a 429 error will be returned.
* "vmTimeout": Denotes the service timeout settings.
* "dbTimeout": Represents the database operation timeout settings.

## # Environment Variables

Environment variables are defined outside the application code, mainly used to alter certain application behaviors without updating the application code.

```
"vars":{
 "*": {"a": "123"},  // Define global shared environment variable
 "g165723": {"a":"456" , "b":"xxx"}, // Define group shared environment variable
 "n1762345": {"a": "ppp"} // Define application environment variable
}
```

In the example above, if fetching environment variables within a nid 1762345 application, the value of variable a is "ppp", if it’s another application but within group ID 165723, then variable a is valued at "456". Otherwise, the variable a is valued at "123".
 # # Operating Procedure

To clearly illustrate the operation steps, the following is a flowchart drawn with Mermaid depicting the configuration process of the config file.

```mermaid
graph TD
    A[Start VisualLogicBase] --> B[Create or Modify config File]
    B --> C[Enter Configuration Information]
    C --> D[Save config File]
    D --> E[Restart VisualLogicBase]
    E --> F[Configuration Takes Effect]
```"
 # Creating and Configuring DBO

### # Add DBO Component

In VisualLogic application development, the dbo component is a tool used to connect to custom databases. Through the dbo component, developers can connect to their deployed MySQL, Oracle, SQL Server, and Postgres databases. A dbo component represents a database instance, and multiple such instances can be added to a project. These instances can be different types of databases, such as adding both MySQL and Postgres instances simultaneously. Multiple databases or schemas can exist under the same instance.

### # Selecting Database Type

After creating a dbo component, you need to choose the type of database to connect to. The currently available types are MySQL, Postgres, SQL Server, and Oracle.

### # Connecting to the Database

After selecting the database type, two methods can be used to achieve the connection: the default connection method and the custom DSN.

#### # Default Connection Method

In the default method, the user simply enters the connection address, username, password, and default DB (the service name in Oracle) to complete the connection. Note that most cloud databases require a whitelist configuration to be accessible, so it's necessary to fill in two VisualLogic backend outbound IPs in the whitelist: 52.83.157.21 and 52.83.192.50. After a successful connection, the database list can be viewed at the bottom of the interface, and the table structure within the database can be further viewed. The default DB will be marked in orange.

#### # Custom DSN Method

Using the custom DSN method, users can input more complex connection strings to include parameters not covered by the default method. The custom DSN method offers the same effect as the default method but provides greater flexibility.

### # Notes on Connecting Different Database Types

- **Postgres**: A default DB must be specified when connecting; otherwise, VisualLogic will default to using "postgres".
- **MySQL and SQL Server**: The default DB can be optionally specified. When specified, tables/schemas can be directly referenced in SQL statements.
- **Oracle**: Does not have the concept of a DB but requires specifying a service name, such as "XE".

### # Local Database Connection

DBO component database connection requests are initiated through the VisualLogic backend server, so the database must be accessible via the Internet. If connecting to an intranet or local database, it is not possible to successfully connect using the VisualLogic editor. However, local databases can be connected and SQL queries executed after private deployment of the application.

### # DBO Usage Scenarios

The DBO component is used in VisualLogic to connect to custom databases. Through the DBO component, users can write SQL within the VisualLogic's service logic to perform database operations, retrieve results, and integrate them with other service logic. To prevent SQL injection and other security issues, VisualLogic requires all SQL to be predefined in SQL templates and not dynamically written within the service.

### # Differences Between DBO and Regular Databases

Unlike other database services provided by VisualLogic, the DBO component essentially acts as an API interface and is not involved in the management of actual database data. The DBO component:

- Does not correspond to specific database entities, so deleting the component does not affect real data.
- Does not distinguish between preview and production tables.
- Puts the responsibility of data backup management on the user.
- Depends on SQL written by the user for all operations.

### # DBO Transactions

The DBO component supports transaction management, allowing transactions to be started, rolled back, and committed directly within the service via the component's methods. Users can embed API requests, function executions, and other logic within DBO transactions.

### # Common Issues with DBO Connection

Since the connections initiated by the DBO component are made through the VisualLogic backend server, the database must be accessible via the public Internet. If the database cannot be publicly accessed, the following methods can be adopted:

1. Skip the connection in the editor and configure SQL directly, then privately deploy the application.
2. Use a privately deployed editor system, deploying the entire VisualLogic editing environment to a local environment.

### # Connection Flowchart

Below is the flowchart for DBO component connection and configuration generated by Mermaid:

```mermaid
graph TB
    A[Add DBO Component] --> B{Choose Database Type}
    B --> C[MySQL]
    B --> D[Postgres]
    B --> E[SQL Server]
    B --> F[Oracle]
    C --> G{Choose Connection Method}
    D --> G
    E --> G
    F --> G
    G --> H[Default Connection]
    G --> I[Custom DSN]
    H --> J[Configure Parameters]
    I --> J
    J --> K[Connection Successful]
    K --> L[View Database List]
    K --> M[Check Connection Notes]
```

By thoroughly understanding and properly configuring the DBO component, developers can fully utilize the flexibility and features of VisualLogic, ensuring efficient and secure access to databases in applications.
 # Execute SQL

## Executing SQL Technical Documentation

### Two Ways to Execute SQL in VisualLogic

VisualLogic offers flexible methods for executing SQL to cater to different application scenarios and needs. Through the following two main ways, you can efficiently manage and operate database data:

#### Method One: Using Actions of the DBO Component

In VisualLogic, the DBO component provides a "Execute Custom SQL" feature, allowing users to enter SQL statements for data operations. During execution, you can choose "Data Query (select)" or "Execution Result (insert/update/delete)" as the data return type. The input SQL statement needs to be in string format to ensure the standardization and security of the data operations.

The notable advantage of this method is its simplicity and intuitiveness, making it suitable for quickly executing predefined SQL operations, allowing users to handle database data directly without writing complex backend code.

#### Method Two: Using SQL Templates

SQL Templates are another flexible SQL execution method provided by VisualLogic, allowing users to manage SQL logic through templates. The specific steps are as follows:

1. **Create SQL Template**: After configuring the DBO connection and ensuring the connection is successful, users can create a new SQL template.
2. **Configure SQL Template**: Define SQL statements, receive parameters, and select the result return type in the template. This method is similar to defining a function, receiving parameters through a specified interface to enhance the dynamics and reusability of SQL statements.
3. **Execute SQL Template**: Select and run the configured SQL template. The system will provide three return parameters: whether the execution was successful, the reason for failure, and the data provided based on the result return type selected in the SQL template.

This method fully leverages the graphical interface and componentized design of VisualLogic, allowing even non-professional programming users to smoothly get started and manage SQL execution. Below is a Mermaid flowchart representing the SQL Template execution process:

```mermaid
graph LR
    A[DBO Connection Configuration] --> B[Create SQL Template]
    B --> C[Add Receive Parameters]
    C --> D[Define SQL Statements]
    D --> E[Select Result Return Type]
    E --> F[Execute SQL Template]
    F --> G{Return Results}
    G --> H[Success]
    G --> I[Reason for Failure]
    G --> J[Return Data]
```

### Advantages and Extensions

VisualLogic is developer-friendly with its visual, modular, and easy-to-use features. Through component-based programming concepts, VisualLogic avoids complex coding, allowing for complex logic operations through a graphical interface. Users can perform componentized development based on business needs, supporting front and back-end integration, third-party library imports, and custom logic extensions, meeting diverse needs from beginners to professional developers.

VisualLogic's methods of executing SQL through components and template systems effectively improve code reusability, development speed, and maintenance convenience, and efficiently integrates various modern application development technologies, such as API calls and data flow processing, providing users with a powerful no-code to low-code development path.

### Related Q&A Supplement

- **Q:** How does VisualLogic ensure SQL Template security?
  - **A:** In SQL Templates, users can define SQL logic through dynamic parameters, but should avoid concatenating parameter values in SQL statements to prevent SQL injection attacks. Ensure parameters are used only in numerical positions, avoiding names identical to field names and SQL keywords.

Through highly componentized and visual design, VisualLogic effectively reduces development complexity, achieving complex applications without extensive handwritten code, providing developers with great convenience. Through its SQL execution functionality, users can flexibly and securely operate databases, providing solid data support for business logic.
 # dbo transaction

### Transaction Execution Process

In VisualLogic, DBO transactions are implemented through DBO component methods, allowing for the embedding of logic into database operations. The basic operations of a transaction include: starting a transaction, rolling back a transaction, and committing a transaction, corresponding to `begin transaction`, `rollback`, and `commit` in SQL.

For example, we can execute two SQL statements: `insert` and `update` in an example. If the `update` operation fails, the transaction will roll back. This allows transaction execution to integrate into our service logic and embed other logical operations such as API requests or function execution.

### Use Cases

DBO transactions are suitable for scenarios where multiple-step database operations need to be composed into a single logical unit, so that if any step fails, the entire operation can be rolled back to ensure data consistency. This is indispensable in scenarios requiring high concurrency and consistency.

### Transaction Creation and Invocation

A transaction is like a group of actions in the background, and its logic can be defined in the transaction event panel. Once a transaction is created, it can be invoked through the service call. When a transaction is called, it can obtain data from outside through the provided parameters and return the result of whether it was successful or not. In case of failure, a rollback operation will be executed.

**Example Flowchart**

```mermaid
flowchart LR
    A((Transaction Start)) --> B[Data Step 1 Commit]
    B -->|Success| C[Data Step 2 Update]
    C -->|Success| D[Data Step 3 Update]
    D -->|Success| E((Transaction Success))
  
    B --> |Rollback on Failure| A
    C --> |Rollback on Failure| A
    D --> |Rollback on Failure| A
```

### Transaction Rollback

Rollback can be triggered automatically or manually:

1. **Automatic Rollback**: The system automatically rolls back a transaction when it detects a data operation failure, such as a commit or update failure.
2. **Manual Rollback**: In some cases, rollback is triggered by special conditions, such as the number of updates must be greater than zero.

### Transaction Internal Implementation and Performance Considerations

- **Removal of Read-Write Separation**: In a transaction, database operations, whether read or write, are executed in the same instance. This avoids data latency issues and improves data consistency.
- **Data Locking**: When updating data rows in a transaction, the system queues the operation to ensure data consistency during execution, though this may sacrifice some performance.

A crucial balance between high performance and consistency lies in transaction design, so caution is advised in design to ensure business needs while considering performance overhead.

### Notes

- The execution time of a transaction is limited (within 5 seconds). If there is too much processing content, it may lead to timeout failure.
- All transaction operations are backend processes and cannot be called directly by the frontend.

### Common DBO Issues

- **Connection Restrictions**: DBO connection requests require public network access to the database. If public network access cannot be achieved, only a private-deployed editor system can be used to solve this issue.
- **Security Configuration**: When necessary, configure the database connection whitelist to allow the IP addresses of VisualLogic backend servers.

These features and capabilities demonstrate VisualLogic's powerful functionality in component-oriented programming, and its flexibility in implementing complex transaction logic in no-code and low-code environments, showing its friendly support for both non-coders and programmers.
 # Four Methods of Database Output

### Overview

In this article, we will introduce four common usage scenarios for database output through the case of food delivery restaurant selection. These applications can be flexibly implemented using the graphical development environment of VisualLogic to meet different filtering needs. This includes outputting in ascending or descending order by data ID, filtering field ranges, advanced filtering conditions, and performing prefix, suffix, and contains queries using keywords.

### Detailed Usage

#### 1. Output in Ascending or Descending Order by Data ID

**Service One:** This method is mainly used to sort and output data in the database. During implementation, the frontend must determine the desired output order and pass corresponding parameters to the backend service.

**Implementation Steps:**

- The frontend confirms the sorting requirements, choosing either ascending or descending order.
- For ascending order, pass the parameter "Ascending 0 Descending 1" as 0, and for descending order, pass 1.
- The backend sets the sorting method based on the input parameter: ascending chooses "Data ID/Creation Time Ascending," while descending chooses descending.
- The output results are assigned to the data object array values for callback function use.

#### 2. Filter Field Range (Text and Numeric)

**Service Two:** This service quickly filters by adding indexes to database fields such as average price per person and star quality. It's important to note the differences between text and numeric fields.

**Implementation Steps:**

- Add indexes to the "Average Price" (numeric) and "Star Quality" (text) fields.
- The frontend selects the average price range, such as 0-100+, by default, it's all, passing in an array in the format [min, max].
- The star quality range is similar, which needs to be passed in as an array containing elements.
- The backend determines the array length from the input to execute the corresponding query conditions.
- The filtered results are passed out via data.

#### 3. Advanced Filtering Conditions "Belongs To"

**Service Three:** This method is implemented using a multi-select dropdown to determine if certain fields belong to a specified set of conditions.

**Implementation Steps:**

- The frontend selects conditions via a multi-select dropdown, passing them as an array to the input parameter "Merchant Features."
- The backend sets filtering conditions to check if the "Merchant Features field belongs to the input parameter."
- Output the filtered results.

#### 4. Begins With/Ends With/Contains

**Service Four:** Use keyword indexing to perform prefix, suffix, and contains queries on strings.

**Implementation Steps:**

- Add an index to the "Restaurant Name" field (only effective for "begins with").
- The frontend selects the search mode (begins with, ends with, or contains) and inputs the keyword.
- Pass the "search mode" and "keyword" to the backend.
- The backend outputs data based on the filtering conditions, ensuring the effective use of indexes.

### Logical Flowchart

```mermaid
graph TD
A[Front-end User Input] --> B{Determine Output Type}
B -->|Ascending/Descending| C[Sort Parameters]
B -->|Range Filter| D[Add Index to Fields]
B -->|Advanced Conditions| E[Select Features]
B -->|Keyword Query| F[Input Search Parameters]
C --> G[Select Sort Order]
D --> H[Input Range Criteria]
E --> I[Check Belong Conditions]
F --> J[Set Search Mode]
G --> K[Execute Output]
H --> K
I --> K
J --> K
K --> L[Return Results via Data]
```

Through the above analysis and flowchart, VisualLogic can be flexibly applied in database filtering and output operations to meet various business and technical needs.
 # Pagination Output/Load Data

#### Overview

In modern application development, pagination and loading are essential features when dealing with large-scale data. This article discusses two primary methods to implement pagination and data loading in VisualLogic: using pagination components and panel components with pull-up loading.

#### Implementation of Pagination Component

The pagination component is a common web navigation tool suitable for web systems and H5 environments. Its primary function is to display large amounts of data in a paged format, avoiding overly long pages that could affect user experience. Here are the specific operational steps for the pagination component:

1. **Add Pagination Component**
   First, select an appropriate parent object in the VisualLogic IDE. Through the extended components option in the toolbar, the pagination component can be added to the layout.
   
2. **Set Pagination Properties**
   In the settings, the total number of data items should be dynamically bound to backend data to display accurate pagination. You can also set the number of items per page and the number of pagination labels according to UI requirements.
   
3. **Implement Data Display**
   Add a "Current Page Change" event to the pagination component, using the event parameters (current page number) to call a service that retrieves the corresponding data. Note that during application initialization, the latest total data count should be set.
   
4. **Optimize Pagination Performance**
   Avoid recalculating the total count each time a page is switched (especially for large datasets), and consider using a separate data table to record the total count.

#### Implementation of Pull-Up Loading in Panel Component

The panel component is used to achieve a seamless pagination loading experience. By implementing pagination through pull-up loading, users can dynamically load more content while scrolling through data.

1. **Panel Configuration and Event Setup**
   The panel height should be fixed, and "Vertical Scrolling" and "Pull-Up Loading" features should be enabled. Set up prompt messages to improve user experience.
   
2. **Control Loading Logic**
   Use a Boolean variable "Is Loading Complete" and a numerical variable "Panel - Current Page" to manage the data loading state and the current page number. When the panel is pulled up, decide whether to call the loading service based on the status of "Is Loading Complete."
   
3. **Set Data Loading State**
   After each data load, set the panel to "Pull-Up Load Complete State" or "No More Data State" to reduce unnecessary service calls.

#### Implementation Process of Pagination Loading

```mermaid
graph TB
A(Start) --> B(Choose Pagination Method)
B -->|Pagination Component| C(Add Component)
C --> D(Set Pagination Properties)
D --> E(Bind Total Data Count)
E --> F(Implement Data Display)
B -->|Panel Component| G(Set Up Panel)
G --> H(Set Pull-Up Events)
H --> I(Control Loading Logic)
F & I --> J(Optimize Performance)
J --> K(End)
```

This article introduces strategies for implementing pagination and data loading in VisualLogic, using a graphical programming approach to reduce the complexity of code writing. By setting reasonable component parameters and event responses, the application's performance and user experience can be effectively enhanced.
 # Batch download images from the database

### 1. Upload Images to the Database

First, upload multiple images to the "Image" database. Ensure that the uploaded image data structure meets the format required for subsequent operations.

### 2. Create Data Objects

In the frontend, create an object array called "data" to store the data batch output from the database. This step requires ensuring that the array structure is consistently bound with the database structure to facilitate subsequent data processing and output operations.

### 3. Set Service Parameters

Create a service and set its input and output parameters:

- Input Parameters: "Start Row" and "End Row". These parameters are used to specify the data download range.
- Output Parameter: "data". This is used to receive the data returned from the database.

The service configuration involves passing the content of the "Start Row" input box to the "Start Row" parameter and the content of the "End Row" input box to the "End Row" parameter when the "Batch Download Images Button" is clicked on the frontend. With this setup, the backend database can output data based on the specified row range, and the output result is assigned to the "data" parameter.

### 4. Database Output Settings

Please note:

- The maximum number of outputs from the database per event is 10,000 records. If not set, the system defaults to a maximum output of 50 records for the database output event.
- The data for published and previewed databases is independent.
- The default row number counting in the database starts at 1.

### 5. Batch File Download

Once the frontend receives the "data" returned from the backend, invoke the "Batch File Download" action via the file interface. At this point, it is necessary to define the custom zip package name and select the image column in the "data" array for download.

Below is the flowchart illustration:

```mermaid
graph TD
    A(Upload Images to the Database)
    B(Create Data Objects)
    C(Set Service Parameters)
    D(Database Output Settings)
    E(Frontend Receives Data)
    F(Invoke Batch File Download Action)

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

By following the above steps, you can seamlessly implement the functionality of batch downloading images from the database using the VisualLogic tool.
 # Set Option List

### Setting Option Lists

In the option list settings of VisualLogic, the dropdown menu has three main modes: one-dimensional array mode, object array mode, and custom mode. Understanding these three modes can help developers display a rich and varied content in dropdown menus for different scenarios, meeting specific business needs.

#### One-Dimensional Array Mode

The one-dimensional array mode is the simplest form of setting up a dropdown menu option list. Developers can directly input options in the "Option List" attribute, separated by commas. For example, entering "Option One, Option Two, Option Three" suffices. It’s crucial not to use Chinese commas as they will cause the system to treat the entries as a single string.

In this mode, the display value and the actual value are the same. That is, if "Zhang San" is displayed, both the "Selected Value" and "Current Selected Value" attributes will be "Zhang San" upon selection. If there is a need for the display content to differ from the actual content, developers should consider using the object array or custom mode.

#### Object Array Mode

The object array mode provides higher flexibility, particularly suitable for scenarios where backend ID-type fields are required, such as student numbers or user IDs. In this mode, developers can set the data source, value field, and display field through the dropdown menu panel. The data source points to an object array, for instance, an array containing "student number" and "name."

Through this mode, a name can be displayed, but the selected value can be the student number. This is achieved by binding the "Value Field" and "Display Field" in the properties panel to the respective fields, separating the display value from the actual value.

#### Custom Mode

The custom mode offers the greatest flexibility, allowing the addition of independent option components under the dropdown menu and dynamically generating options in conjunction with a for-loop container. This mode supports the independent configuration of each option’s properties, such as text style or whether it is disabled and allows for option grouping and custom style settings.

Through the custom mode, developers can finely control the displayed text of each option, the actual value of the options, and flexibly use grouping and styling functions to fully showcase the diversity and complexity of the options.

### Functional Summary

The VisualLogic tool offers various option setting methods, providing developers with convenient and flexible option configuration means. In real-world applications, developers can freely choose the appropriate mode according to their needs, significantly improving development efficiency and application responsiveness. With the VisualLogic tool, anyone can easily get started building complex application logic.

### Flowchart

The following is an overview of the process for different option settings:

```mermaid
graph TD
    A[Start Setting Option List] --> B{Select Mode}
    B --> C[One-Dimensional Array Mode] --> D[Input Option List]
    B --> E[Object Array Mode] --> F[Configure Data Source]
    F --> G[Select Value Field and Display Field]
    B --> H[Custom Mode] --> I[Add Dropdown Menu Option Component]
    I --> J[Combine With For-Loop Container]
    J --> K[Configure Independent Option Properties]
    C --> L[Display Value Equals Actual Value]
    E --> M[Display Value and Actual Value Separation]
    H --> N[Configure Different Styles and Groupings]
```

By combining these three modes and VisualLogic’s powerful component-oriented programming design philosophy, developers can efficiently and intuitively set dropdown menu option lists, significantly enhancing development efficiency and the flexibility of feature implementation.
 # Generate Password

### Password Generation and Verification

In modern application development, the security of user passwords is crucial. VisualLogic, as a universal graphical programming language along with its complete IDE environment, offers convenient encryption components to achieve password generation and verification, thereby greatly enhancing the security of user data. The core of this functionality lies in the encrypted storage of user information and providing a method to convert user plaintext passwords into encrypted strings. This ensures that even if the database is breached, sensitive user data remains secure.

#### Password Generation and Verification

The encryption component supports both password generation and verification, and these functionalities are implemented by simply calling methods on the component object without complex configurations. This not only simplifies the development process but also increases development efficiency.

1. **Password Generation**: A plaintext password entered by the user, such as "abc123", can be converted into an encrypted string through the password generation method. For example, the generated string might look like "$SGbhFOO5QGnJq9ABi8O1zE6vnqhtQecF.Fdo1I1bxsEcsH9N1/c". This method means that the user's original password is not stored directly in the database but is stored in an encrypted form, so even backend administrators cannot know the original password, enhancing information security.

2. **Password Verification**: When the user logs in or performs operations that require password verification, the plaintext password entered by the user is compared with the encrypted password in the database. By using the password verification method, the system will determine whether the plaintext password entered by the user matches the stored encrypted password after the same encryption process. If they match, the verification passes; otherwise, it fails.

During implementation, since the ciphertext usually contains special characters, it is recommended to enclose the ciphertext in quotes when operating in the formula editor. However, if variables are used for password operations, no additional processing is needed.

##### Operation Process

```mermaid
graph TD;
    A[Start] --> B[User enters plaintext password];
    B --> C[Generate encrypted password];
    C --> D[Store encrypted password in database];
    D --> E{User attempts to log in};
    E -->|Yes| F[User enters plaintext password];
    E -->|No| G[End];
    F --> H[Verify password with stored ciphertext];
    H --> I{Verification result};
    I -->|Success| J[Login successful];
    I -->|Failure| K[Login failed];
    J --> G;
    K --> G;
```

This process provides users with a secure and convenient password management method, effectively preventing the risk of information leakage. VisualLogic, through its powerful and flexible component-based approach, makes the entire process intuitive and easy to understand, suitable for developers of varying technical levels.

With the component-based programming concept of VisualLogic, developers can achieve rapid development without code, and for scenarios requiring code implementation, VisualLogic also supports embedding mainstream programming languages, greatly expanding its application flexibility and scalability. These features make VisualLogic an attractive choice in password security management and application development.
 # Generate and Verify Passwords

### Generation and Verification of Passwords

In modern information security, password management is crucial. To ensure the security of user passwords, effective encryption mechanisms must be adopted. The encryption component provided by VisualLogic includes functions for password generation and verification, offering users a secure and convenient password handling solution. The following is a detailed technical explanation:

#### Component Overview

The main function of the encryption component is to encrypt and verify passwords. Whenever a user enters a password, the system will encrypt it through the encryption component and store the encrypted value. This ensures that even if the database is compromised, the user's password is effectively protected from direct retrieval.

* **Password Generation Function**: When a user inputs a plaintext password, the system will encrypt it into a complex string. This encryption process cannot be directly reversed, ensuring the security of the password. For example, inputting the plaintext "abc123" will output an encrypted string like "$S$DZhblFOO5QGnJq9ABi8O1zE6vnqhtQecF.Fdo1I1bxsEcsH9N1/c".
  
* **Password Verification Function**: When a user attempts to log in or perform an operation that requires password verification, the system will compare the plaintext password entered by the user with the stored encrypted password. The comparison process returns a boolean value, with true indicating successful password verification and false indicating failure.

The VisualLogic encryption component does not require any complex configuration; it only requires calling the corresponding methods in the service. This allows users, even those without a development background, to easily implement password generation and verification functions.

#### Advantages and Details

1. **Security**: By using password generation and verification functions, the passwords stored in the database are encrypted, reducing the risk of information leakage. Even backend administrators cannot view users' original passwords.

2. **Ease of Use**: The component is designed following the "component-based programming" concept. Users can complete encryption and verification simply by calling relevant methods. In the password verification process, only the plaintext password input by the user and the stored encrypted password need to be compared, with no additional processing steps required.
   
3. **Flexibility**: During usage, the encrypted password may contain special characters. To ensure normal recognition by the formula editor, it is recommended to add quotes when directly inputting. However, if it is a variable reference, no such handling is required.

4. **Convenience**: The "componentized mindset" is fully embodied in the application of VisualLogic. Each function is particularly standardized, allowing users to quickly become familiar and apply it to actual development scenarios. The characteristics of no-code development and support for automatic generation of front-end and back-end code also make VisualLogic highly adaptive among different user groups.

#### Operational Flow Chart

```mermaid
graph TD;
    A[User Inputs Plaintext Password] --> B[Call Password Generation Function];
    B --> C[Generate Encrypted String];
    C --> D[Store Encrypted Password in Database];
    E[User Attempts to Log In or Verify Operation] --> F[User Inputs Verification Plaintext Password];
    F --> G[Retrieve Encrypted Password from Database];
    G --> H[Call Password Verification Function];
    H --> I{Password Verification Result};
    I -->|Verification Successful| J[Allow Access];
    I -->|Verification Failed| K[Deny Access];
```

The above flow illustrates how the VisualLogic encryption component can generate and verify user passwords. Throughout the process, there are no complex operational steps, only the need to call the methods provided by the component, reflecting full user-friendliness and security.
 # WeChat Authorization Login

### VisualLogic中的微信授权功能

VisualLogic默认提供了微信公众号授权登录的功能，用户可以通过配置中心开启授权功能，以便在应用初始化时进行授权。这一过程可选是否获取用户的头像和昵称。当开启此选项时，应用启动后会跳转至授权页面，用户可能会遇到短暂的白屏和“登录中”的加载提示。如果选择获取头像和昵称，用户还将看到一个授权提示窗口。

然而，为了提供更加灵活的用户体验，在某些场景下，我们希望仅在需要时触发授权过程。这时候，可以借助VisualLogic的后台微信组件来实现微信的异步授权服务。

### 实现异步授权的步骤

1. **配置接口：** 首先，在接口配置中启用微信接口，并输入微信公众号的appid和app secret。切记不要开启授权。需要注意的是，进行异步授权必须使用自定义的微信公众号，网页授权域名要相应设置为`v4rel.h5sys.cn`或`v4pre.h5sys.cn`（如果使用预览地址测试）。
2. **添加前台组件：** 在前端页面中，添加“微信公众号”组件。通过触发“跳转授权页”的动作，用户界面会重定向到应用地址，并携带URL参数`code`。随后，可以在页面中展示获取到的`code`。
3. **获取用户信息：** 在后台，添加微信组件，通过获取的`code`来获取用户信息，包括用户的头像、昵称、openid等。
4. **处理access_token：** 由于每个`code`只能使用一次来获取用户信息，因此需要缓存access_token。如果需要再次获取用户信息，可以通过access_token来检索，前提是需要传入当前用户的openid。后台微信组件提供了直接获取access_token的方法。

```mermaid
graph LR
A[配置微信公众号] --> B[启用微信接口]
B --> C[添加前台微信公众号组件]
C --> D[通过code获取用户信息]
D --> E[缓存access_token]
```

### VisualLogic和微信公众号平台的配置说明

在使用VisualLogic进行开发时，如果H5应用部署在微信平台且需要获取用户的昵称和头像，需在配置界面勾选“开启授权”。若自有公众号，可以在配置界面中填入公众号的appID和appSecret，并设置自定义授权域名（例如`v4rel.h5sys.cn`）。

如果在微信中出现错误页面，可能是因为微信公众号后台的域名配置不当。解决此问题的方法是，在微信公众平台上设置合法域名，包括业务域名和JS接口安全域名的配置，这一步只对企业版公众号开放。

### 自定义授权域名的意义

使用自定义授权域名可以节省网页授权域名的空间，因为单个微信公众号下的网页授权域名限制为三个。因此，建议在使用自定义公众号时，启用自定义授权域名功能，以避免授权域名下的所有作品因违规操作导致无法访问。

为了使用自定义域名，需在配置后台DNS指向`v4rel.h5sys.cn`。设置时需确保应用发布的域名和授权域名一致，或者指向一个云服务商配置的标准名称，以保证访问的稳定性和安全性。

### 微信接口的相关配置

在微信接口配置中，需要绑定服务号并修改相关设置。通过为服务号配置绑定的appid和appsecret以及设置授权域名，可以确保在微信中运行的案例正常访问，从而实现与微信服务号的无缝连接。

通过这些详细的步骤和配置，使得VisualLogic在进行微信授权登录时展现了其强大的组件化和灵活性优势，使开发人员可以方便地实现各种登录场景应用。"
 # WeChat Pay

### WeChat Payment Technical Documentation

#### Payment-Related Configurations
To successfully use WeChat Payment in a VisualLogic environment, we need to complete the application and configuration of the WeChat application. This involves various types of applications such as WeChat Official Accounts, Mini Programs, and WeChat Apps. The registration and configuration guide are as follows:

1. **WeChat Official Account (Service Account)**
   - Application Address: [Click to register](https://mp.weixin.qq.com/)
   - Registration Process: [Registration Guide](https://kf.qq.com/faq/120911VrYVrA151013MfYvYV.html)
   - WeChat Authentication Process: [Authentication Guide](https://kf.qq.com/faq/120911VrYVrA151013zu63u6.html)
   - After completing registration and authentication, you can configure the service account for use in VisualLogic.

2. **WeChat Mini Program**
   - Registration Address: [Click to register](https://mp.weixin.qq.com)
   - Registration Process: [Registration Guide](https://kf.qq.com/faq/170109iQBJ3Q170109JbQfiu.html) (Organization type: Enterprise)
   - WeChat Authentication Process: [Authentication Guide](https://developers.weixin.qq.com/miniprogram/product/renzheng.html)
   - Ensure enterprise authentication is completed to use payment functions.

3. **WeChat Mobile Application**
   - Registration Address: [Click to register](https://open.weixin.qq.com/)
   - Follow the WeChat App documentation to create and configure necessary information.

Next, we illustrate how to enable WeChat Payment for a native app.

#### Enabling WeChat Payment
1. Log in to the WeChat Open Platform and enter the created mobile application.
2. Go to the merchant platform, link the necessary mobile app appid, and enable the APP payment function.

WeChat Payment Usage Demo:
- [WeChat H5 Payment Demo](https://www.ivx.cn/ih5/app/template/download?id=4115)
- [WeChat Mini Program Payment Demo](https://www.ivx.cn/ih5/app/template/download?id=4116)
- [WeChat APP Android Payment Demo](https://www.ivx.cn/ih5/app/template/download?id=4121)

Notes:
- WeChat H5 Payment: In the "JSAPI Payment" module of the product center on the merchant platform, you need to fill in the authorized directory.

#### Payment Process
The WeChat payment process generally involves six key steps, applicable to Official Account H5, Mini Program, and native APP:

1. **Generate Payment Order**
   - The backend WeChat component generates a payment order and returns payment information.

2. **Frontend Initiates Payment Interface**
   - According to the generated payment information, the payment interface is initiated for the payment process.

3. **User Completes/Abandons Payment**
   - The user interacts directly with the WeChat application, and the payment result is confirmed via service notification, not frontend callback.

4. **Backend Receives WeChat Payment Result**
   - The configured backend service receives WeChat's payment notification and updates the order status.

5. **Active Order Status Query**
   - The backend queries the actual payment status based on the order number to ensure accurate information.

6. **Refund Process**
   - A refund request is initiated using the order information. If processed successfully, the refund amount is returned to the user.

```mermaid
graph TD;
    A[Apply for WeChat Application] --> B[Generate Payment Order];
    B --> C[Frontend Initiates Payment Interface];
    C --> D[User Completes/Abandons Payment];
    D --> E[Backend Receives Payment Notification];
    D --> F[Active Order Status Query];
    F --> G[Refund Process];
```

#### WeChat APP Functionality
The primary role of the WeChat APP is to trigger the WeChat SDK through iOS and Android native applications, implementing functionalities such as payment, sharing, and login.

Configuration Guide:
- Go to the WeChat Open Platform to create an application and obtain the APP ID.
- Fill out the created application information in the WeChat configuration of VisualLogic.

#### Product Advantages
By utilizing WeChat Payment through VisualLogic, developers can not only quickly implement payment functionalities but also lower the development threshold through graphical programming. The modular programming and flexible frontend-backend development support of VisualLogic make implementing WeChat Payment easier. By supporting different platforms (such as Vue, React, Java SpringBoot), it further simplifies the cross-platform development process for payment functionalities.

Supplementary Q&A:
- Ensure to add the WeChat security domain of the Official Account and Mini Program during the VisualLogic configuration to avoid common preview or payment errors.
- If errors occur during the payment process, check whether the merchant platform's whitelist and JSAPI payment authorized directory are properly configured.

Overall, VisualLogic, by providing user-friendly components and detailed configuration documentation, makes WeChat Payment integration efficient and accurate, suitable for developers with low code sensitivity.
 # Alipay Payment

### Using VisualLogic Backend Alipay Component

In a VisualLogic application, the Alipay payment functionality mainly relies on the backend Alipay component. By following detailed configuration and development steps, you can implement Alipay payment on mobile web pages. Below is the user guide and related process:

#### I. Log in to Alipay Open Platform and Create Application

1. **Application Creation and Configuration**
   Log in to the Alipay Open Platform and create a new application. On the application's detail page, add the "Mobile Website Payment" feature from the capabilities list. The default contract status is "Inactive," and you need to click to sign the agreement. Upon redirecting to the Alipay Merchant Center, upload the business license (the legal representative must match the Alipay account). After approval, you can use this feature. Finally, bind the application's appid in the Alipay Merchant Center and ensure the status is "Active."
2. **Interface Encryption Configuration**
   In the development settings of the Alipay Open Platform, configure public key encryption. You need to generate a set of public and private keys yourself (it is recommended to use RSA2, PKCS1 format) and fill in the application public key back to the Open Platform while saving the application private key.
3. **Application Lifecycle Approval**
   The application must be submitted to the Alipay Open Platform and go through an approval process before it can fully use the payment function. Note that the application name should not contain words like "Test," as it may lead to approval failure.

#### II. Configure Alipay Payment on VisualLogic Platform

1. **Add Alipay Component** 
   In the VisualLogic backend, add the Alipay payment component and fill in the following information:

   - **APPID**: The appid obtained in the first step.
   - **Public Key**: The Alipay public key obtained in the first step (not the application public key).
   - **Private Key**: The application private key generated in the first step.
2. **Configure Payment Links and Notification Address**
   Modify the callback address and asynchronous notification address in the payment component to track and respond to payment results.
3. **Payment Flow**
   Users access the VisualLogic application through a mobile browser and select the payment option. Upon successful payment, Alipay will return all related parameters. You can add database fields in VisualLogic to collect these parameters (e.g., order number, amount, buyer's Alipay account, transaction status) for query and management purposes.

```mermaid
flowchart TD
    A[Log in to Alipay Open Center] --> B[Create Application]
    B --> C[Add Mobile Website Payment Feature]
    C --> D[Upload Business License and Approve]
    D --> E[Bind Application appid and Confirm Activation]
    E --> F[Configure Public Key Encryption]
    F --> G[Generate and Save Public and Private Keys]
    G --> H[Submit Application for Approval]
    H --> I[Add Alipay Component to VisualLogic]
    I --> J[Configure APPID, Public Key, Private Key]
    J --> K[Modify Callback Address and Notification Address]
    K --> L[Enable Alipay Payment]
    L --> M[Complete Payment and Parameter Collection]
```

### Frequently Asked Questions

#### Q&A

**Q: How to handle payment rejection or failure?**
A: Check the public key configuration in the Alipay Open Platform to ensure correctness; verify the binding status and configuration permissions in the Alipay Merchant Center.

This document provides a complete technical route to implement Alipay payment through VisualLogic, from platform configuration to payment execution. It is convenient for technical personnel to quickly get started and also serves as a clear visual explanation for users who are not familiar with coding.
 # Generate Rich Text Table of Contents

### Introduction

In modern application development, rich and dynamic text content is essential, which is also why rich text and its editors are highly popular. The VisualLogic platform offers a powerful rich text feature that provides an easy way to generate and manage rich text directories, and supports extracting directory information directly from documents. Below is a detailed introduction on how to use the rich text tools of the VisualLogic platform to generate and optimize rich text directories.

### 1. Automatically Generate Document Directory

The rich text and rich text editor provided by the VisualLogic platform support extracting directory information from documents automatically. During initialization, developers can call the "Get Subtitles List" action to generate an array of objects that includes subtitle information. This array contains the following fields:

- **id**: The unique identifier of the subtitle, used for navigation within the page.
- **text**: The text content of the subtitle, displayed in the document.
- **type**: The type of subtitle, represented by a number, with different numbers corresponding to different levels of headings (such as header1, header2, etc.).

Developers can display these subtitles using a loop list and adjust their display styles according to different heading levels.

**Note**: In practical applications, rich text content is usually fetched from a database. In such cases, subtitle lists should be fetched after successfully obtaining the content.

### 2. Navigate to Subtitles

To achieve navigation when a user clicks on the directory, the VisualLogic platform provides the "Navigate to Subtitle" action. Since there might be duplicate subtitle texts, it is necessary to specify the subtitle ID for accurate navigation.

The steps to implement this functionality are as follows:

- Create an action group to handle subtitle navigation. Pass the corresponding subtitle ID when calling the action.
- To achieve click highlight effect, you can create a variable to record the current ID and adjust the directory text color based on this ID.

### 3. Manually Scroll Document, Directory Automatically Highlights

In addition to the click navigation function, VisualLogic also supports automatic highlighting of the current directory item in view when the user manually scrolls. This function is achieved through the scroll event of the rich text and real-time acquisition of subdirectory position.

Steps are as follows:

- Obtain the directory position array. In this array, the data includes not only basic subtitle information but also the **offsetTop** field, which indicates the distance from the current directory to the top edge of the rich text.
- Determine the subtitle closest to the top in the current view through filtering and sorting operations.
- Assign the ID of this subtitle to the current highlight ID to achieve automatic highlight effect.

### 4. Record Browsing Position and Implement Automatic Navigation

To enable users to return to the previously viewed position after a page refresh or close, the current subtitle information can be recorded in the URL parameters. Steps are as follows:

- When the user scrolls or clicks on a subtitle, add the current subtitle text as a hash parameter in the URL.
- When the page loads, read the URL parameter, get the corresponding subtitle ID, and automatically navigate to that position.

### Preview and Download

To experience the rich text directory function and view specific implementation cases, please visit the following links:

- [Demo Preview Address](https://file4ec59ae07148.v4.h5sys.cn/play/V9AUOMg1)
- [Demo Download Address](https://www.ivx.cn/ih5/app/template/download?id=4093)

### Visual Flowchart

```mermaid
graph TD;
A[Initialize Page] --> B[Get Rich Text Content];
B --> C[Get Subtitles List];
C --> D[Display Directory List];
D --> E[User Event Listener];
E -->|Click| F[Navigate to Subtitle];
E -->|Scroll| G[Update Highlight Directory];
F --> H[Update URL Parameter];
G --> H;
H -->|Page Refresh| C;
```

With these functionalities, the VisualLogic platform allows developers to efficiently generate, manage, and display dynamic rich text directories, enhancing content presentation and user interaction experience.
 # Tencent Cloud k8s Elastic Container Practice

## How to Deploy a VisualLogic Application on Tencent Cloud K8s Elastic Containers

In this chapter, we will briefly introduce how to deploy a VisualLogic application to container services. A container (docker) is a more granular virtual computing unit than a server, offering better resource efficiency and scalability, making containerization increasingly prevalent in large-scale enterprise application deployments.

To run a VisualLogic application, we need to prepare the following resources:

- **Object Storage (oss/s3)**
- **Cloud Database RDS**
- **Cloud Database Redis**

The container service itself is stateless (i.e., does not store application data), so it needs to connect to shared file (object storage) and database services (cloud database RDS).

Using Tencent Cloud's Elastic Container as an example, here are the detailed steps:

### A. Create a New Elastic Cluster

1. Access the Tencent Cloud product "Container Service" console.
2. Create a "Elastic Cluster".
3. Fill out the "Cluster Name", select the appropriate "Region", choose the recommended "Container Network", and click "Complete".

Wait for 1-2 minutes, and once it's successfully created and "Running", click on the code above the name to enter the cluster interface. At this stage, it is important to note that the configuration file method for "Container Service" is different from that of "Cloud Server". The cloud server’s configuration file is stored in the config.json file, which needs to be edited when deploying applications nid; however, in container deployment, this is passed externally through an environment variable. Here, the environment variable name for the VisualLogic base is: CONFIG_JSON. The online version of the VisualLogic base will automatically search for the environment variable named "CONFIG_JSON" to obtain configuration-related information.

### B. Create a New ConfigMap Configuration Item

First, switch to "Config Management" in the established cluster and create a new configuration item in "ConfigMap".

1. Name it arbitrarily, such as `ivx-config`.
2. Arbitrarily name the variable, such as `ivx-config-detail`.
3. The variable value must be in standard JSON format specifying the addresses for oss, mysql (RDS), and redis databases. These addresses must be public service addresses accessible by the current elastic container.

The variable value template is as follows:

```json
{
  "oss": "oss address",
  "mysql": "mysql address",
  "redis": "redis address"
}
```

Click "Create ConfigMap" to succeed.

Note: The `loadPackage` field in `dir` is set to zip: it indicates that the `nidxxxx.zip` package exported from private deployment needs to be uploaded to the zip folder under the pathPrefix in the object storage bucket.

### C. Create a New Workload (Deployment) and "Restart" Load

1. Switch to "Workload" within the cluster and click "New" in "Deployment".
2. Fill in any name, such as `ivx-test`.
3. Fill in the "Instance Container" name such as `ivx-test`, "Image", and "Image Version" (this information can be found in the editor by clicking on private deployment and switching to "Docker Version"), and set environment variables based on the ConfigMap configuration established in the second step.

In the same interface, select "Provide Public Access", set port mapping to 80, and click "Create Workload". Once successful, you will see the workload in a `running` status. You can access the application through the path shown in logs, such as: `/play/9kHkBLTy`.

If you need to modify the config information, such as adding multiple nid applications to deploy to the elastic container, update the variable value and click complete, then go to "Deployment" in "Workload" under the "Elastic Cluster" to update the "Pod quantity": set it to 0, click update instance quantity, reset it to 1, and update instance quantity once more to achieve a similar restart effect.

```mermaid
graph LR
A[Enter Container Service Console] --> B[Create Elastic Cluster]
B --> C[Fill in Cluster Information]
C --> D[Create ConfigMap]
D --> E[Fill in Variable Values]
E --> F[Create Workload]
F --> G[Provide Public Access]
G --> H[View Application Access Path]
```

### D: Use HTTPS + Domain Access

1. Switch to "Service and Routing" under "Ingress" and create a new entry.
2. Enter the name and use a domain with an existing SSL certificate. After successfully creating it, add a host record in the domain management backend, set the record type to A, and record value to the IP in Ingress (refer to the domain management backend's setup document). Wait a few minutes for DNS to update, then access via "http://+domain+case path".

Then modify this Ingress configuration to allow access via https:

1. Select "https:443" and add forwarding rules.
2. Create a new key and jump to create a server certificate (follow the process to apply for a certificate).
3. After the certificate is issued successfully, return to "Update Forwarding Configuration", select the newly created key name and server certificate, and click "Update Forwarding Configuration". You can successfully use the https domain to access.

### E: Configuration of NAT Gateway for Opening in WeChat

1. Log in to Tencent Cloud, open the "NAT Gateway", switch the region to the one where the elastic cluster is located, and create a new one.
2. Enter the gateway name and select the associated network (default selected), click create (keep the default "Create Elastic IP").
3. Switch to the route table, click the name part of the existing route table, add a new routing policy, fill in the destination end with `0.0.0.0/0`, select "NAT Gateway" for the next type, and select the newly created NAT Gateway.
4. Return to the NAT Gateway, check the elastic IP, and add it to the public account's "IP whitelist".

At this point, the link to the case can be opened on WeChat.

Following the above steps, you can access the deployed VisualLogic application on Tencent Cloud k8s elastic container instances through WeChat, domain name, or public IP.
 # Send template message (subscription message)

## Sending Template Messages (Subscription Messages)

### Mini Program Subscription Messages

Using the VisualLogic backend WeChat components allows developers to easily send Mini Program subscription messages to users. Users will receive customized notifications when certain subscribed events occur. For example, when a user successfully registers for an event in the Mini Program, the system can automatically push a "registration successful" notification to the user's end. The specific usage steps for this function are as follows:

1. **Log in to the WeChat Developer Platform**:
   Go to the [WeChat Developer Platform](https://mp.weixin.qq.com) and log in. Enter and find the "Subscription Messages" module.

2. **Choose an Appropriate Template**:
   In the Subscription Messages module, select the message template that best matches the application scenario and check the keywords that need to be sent to the user.

3. **Transfer Content and Send Message**:
   Obtain the template content from the above steps and pass it to the "Send Mini Program Subscription Message" interface to successfully send the message to the user's Mini Program.

### Official Account Template Messages

Similarly, VisualLogic's backend WeChat components support sending Official Account template messages. These messages are usually sent through H5 pages to users who have already followed the service account. The steps for implementing Official Account template messages are as follows:

1. **Log in to the Official Account Platform**:
   Visit the [Official Account Platform](https://mp.weixin.qq.com), open the "Template Messages" module, browse and select an appropriate template from the template library to add.

2. **Call Interface to Send Messages**:
   Input the user's `openid` (ensure the user has followed the service account), the template ID, and the specific template content. After completing these steps, call the send Official Account template message interface to send the message.

#### Message Sending Flowchart

```mermaid
graph TD
    A[Log in to WeChat Developer Platform] --> B[Enter Subscription Messages Module]
    B --> C[Choose a Message Template]
    C --> D[Transfer Template Content]
    D --> E[Send Mini Program Subscription Message Successfully]

    F[Log in to Official Account Platform] --> G[Enter Template Messages Module]
    G --> H[Choose a Message Template]
    H --> I[Input User Information]
    I --> J[Call Send Message Interface]
    J --> K[Send Official Account Template Message Successfully]
```

### User Subscription Notification Confirmation

In actual development, it's necessary to understand whether a user has subscribed to Mini Program notifications. Using WeChat Mini Program's message subscription request can pop up a dialog allowing the user to choose to subscribe. After the user completes the subscription selection, developers can obtain the subscription ID list from the event panel, which specifically records whether the user accepts each template message subscription.

#### Example:

- The user clicks accept or reject in the message subscription dialog
- You can obtain an object from the event panel like `{"2ydjfdahdlafhgckk":"accept","jdyhgdaldgg":"reject"}`

This function effectively reduces information redundancy when users receive unnecessary notifications, ensuring user experience.

Through the above framework, VisualLogic provides a simple and flexible way to help developers achieve no-code or low-code management of WeChat subscription messages and template messages. Developers can use graphical logic construction and a rich component library to efficiently build the required application functions.
 # Application Architecture

### Overview of Application Architecture

VisualLogic is a programming language based on general graphical representation, adopting the "component-oriented programming" design philosophy. It allows developers to build applications through simple drag-and-drop of components, and its powerful development environment supports rapid prototyping and no-code implementation. This article will analyze in detail the application architecture of VisualLogic, helping you quickly grasp its technical background and advantages.

#### Separated Frontend and Backend Architecture

The core of VisualLogic's application architecture lies in its separated frontend and backend design. The logic of the frontend and backend is independent and communicates through service components. This design simplifies application development and maintenance while improving system performance and security. The frontend application is responsible for displaying the client interface and interaction, while the backend handles data processing and logic operations.

```mermaid
graph TD
    subgraph Frontend
        direction TB
        A[Frontend Application]
    end
    
    subgraph Backend
        direction TB
        B1[Service1]
        B2[Service2]
        B3[Service3]
        D[Backend Resource Interface]
    end
    
    A --> B1
    A --> B2
    A --> B3
    B1 --> D
    B2 --> D
    B3 --> D
```

The frontend part covers web applications, mini programs, native apps, and Windows exe applications. The frontend programs run locally, while backend resources are handled by server-side programs that manage backend data and communication logic.

#### Backend Resource Interfaces and Service Logic

The backend resource interfaces mainly include components like database operations, APIs, caching, and files. VisualLogic itself generates applications but does not provide backend resources. This means that to use backend features in VisualLogic applications, users need to provide their own resources, such as MySQL databases, Redis, etc.

Service logic provides HTTP service interfaces (REST API) that the frontend can call. Additionally, VisualLogic's backend services can directly operate various backend resources and define internal handling logic, which is especially user-friendly for beginners in no-code development.

#### Example of Application Architecture

To specifically illustrate the application logic of VisualLogic, below is an example of a simple form submission application:

```mermaid
graph LR
    subgraph Frontend Interface
        direction TB
        B[Submit Button]
    end

    subgraph Service
        direction TB
        C[Submit Service]
    end

    subgraph Backend Resources
        direction TB
        D[Database Table]
        D1[Data ID]
        D2[Name]
        D3[Phone]
    end

    B --> |Call Service| C
    C --> |Write Data| D
    D --> |Return Result| C
    C --> |Return Result| B

    D --> D1
    D --> D2
    D --> D3
```

In this example, users submit information through the frontend interface, and this data is transmitted to the backend database for storage via the 'Submit Service,' with the processing results returned for display on the frontend application.

#### Support for Micro-Frontend Architecture

VisualLogic supports micro-frontend architecture and allows communication between different sub-applications through an event bus. Such a design not only allows large applications to be broken down into more manageable sub-applications but also enables dynamic loading and independent deployment, enhancing development flexibility and efficiency.

#### Application Runtime Environment and Deployment Structure

The deployment structure of VisualLogic applications is consistent in both testing and production environments. During deployment, each project is packaged as an independent Docker image and runs in the Kubernetes service.

```mermaid
graph LR
    A[User Request] --> B[Load Balancer]
    B --> C[Business Application]
    C --> D[User Center]
    C --> E[Service Registry]
    C --> F[Log Service]
    F --> G[Database Service]
    F --> H[File Storage]
```

In this architecture, VisualLogic applications share the user center and distributed transaction center, while most other functional modules are encapsulated within the application to reduce service coupling.

#### Support for Public Cloud Resources

VisualLogic's public cloud architecture uses AWS services, supporting one-click publishing and deployment without the need for users to configure backend resources. This greatly reduces deployment difficulties, making VisualLogic a lightweight and efficient development tool.

On the public cloud, all resources support elastic scaling to ensure optimal performance in any usage scenario, and resources shared among users further reduce the costs.

#### Advantages and Summary

The component-based architecture of VisualLogic not only enables "no-code" development but also achieves efficient development and deployment through flexible backend connections. Whether for individual developers or enterprise users, VisualLogic provides a more convenient, secure, and scalable development experience. It is an ideal tool for users looking to quickly build applications, significantly enhancing development efficiency and application robustness.
 # WeChat Interface Configuration

## WeChat Interface Configuration

### Objective and Preparation

In the previous chapter, we implemented accessing via an HTTPS domain in a PC browser, such as the format "https://xxx.com/xxx". However, this does not work in the WeChat browser because WeChat requires the domain to be authorized by a Service Account for access. Therefore, it is necessary to bind the case to a Service Account and modify the relevant settings of the Service Account.

### Service Account Settings

1. Log in to the Service Account backend (https://mp.weixin.qq.com/)
   - Click on "Basic Configuration" to obtain the Service Account's appid and appsecret (you can reset it if you've forgotten the appsecret), and add the server's IP address to the "IP Whitelist".

2. Switch to "Function Settings" under "Official Account Settings"
   - Fill in the root domain name for both the Business Domain and JS Secure Domain (note: you can only modify the Business Domain and JS Secure Domain five times per month).
   - The Web Authorization Domain should be the secondary domain that needs to be accessed. For instance, if "https://wxauth.ih5school.cn/play/asIPshax" is the target, fill in wxauth.ih5school.cn as the Web Authorization Domain.

The above are the settings for the WeChat Official Account (Service Account).

### VisualLogic Configuration Center Binding Service Account

1. Enter the Service Account's appid and APP secret into the WeChat interface configuration in the Configuration Center of VisualLogic, and fill in the Custom Authorization Domain with the secondary domain (consistent with the Service Account's Web Authorization Domain from the previous step).
2. After saving, export the case again and deploy it privately to the server. This enables successfully opening the case in the WeChat browser, such as "https://wxauth.ih5school.cn/play/asIPshax".

Below is the flowchart of the WeChat interface configuration process:

```mermaid
graph TD
  A[Login to Service Account Backend]
  B[Click Basic Configuration]
  C[Obtain appid and appsecret]
  D[Add server IP address to whitelist]
  E[Switch to Official Account Settings]
  F[Fill in Business Domain and JS Secure Domain]
  G[Fill in Web Authorization Domain]
  H[VisualLogic Configuration Center Binding Service Account]
  I[Enter appid and APP secret]
  J[Custom Authorization Domain]
  K[Save and Export Case]
  L[Deploy to Server]

  A --> B
  B --> C
  C --> D
  D --> E
  E --> F
  F --> G
  G --> H
  H --> I
  I --> J
  J --> K
  K --> L
```

### Supplementary Content

According to the QA data we consulted, the following supplementary content applies:

1. Ensure that the Web Authorization Domain and the secondary domain filled in the Service Account configuration are consistent during the WeChat interface configuration process to avoid authorization failures.
2. If encountering issues with the WeChat Service Account configuration, refer to the official WeChat documentation or contact technical support for inspection.

### Conclusion

By following the steps above, we can effectively configure the WeChat interface settings to ensure that HTTPS domains configured in the PC browser can be accessed normally in the WeChat browser. This configuration process is equally applicable in the VisualLogic Configuration Center and ensures the application can be done without writing complex code during the development process.
 # Baota Deployment

# Deploying Baota

## 1. Install Baota on the Server

### Installing Baota

After logging into the server (such as Alibaba Cloud ECS and Tencent Cloud CVM), the following interface is displayed:

```shell
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```

Follow the on-screen prompts to obtain the latest version installation instructions from the Baota official website. After successful installation, a link address along with the username and password for the Baota panel will be displayed for login.

### Open Server Ports

The process of opening ports on different servers can be viewed through external links: [Tencent Cloud](https://www.bt.cn/bbs/thread-1229-1-1.html), [Alibaba Cloud](https://www.bt.cn/bbs/thread-2897-1-1.html), [Huawei Cloud](https://www.bt.cn/bbs/thread-3923-1-1.html)

After logging into the management website of the service provider, open the following ports:

- Ports 8888, 20, 21, 39000-40000 (Linux system)
- 3000-4000 (Windows system)
- 22 (SSH), 80, 443 (websites and SSL), 3306 (database remote connection), 888 (phpMyAdmin)

**For example, opening port 8888 on Alibaba Cloud:**

Once port 8888 is opened, the Baota panel can be accessed. Detailed installation tutorials for Baota can be found on the Baota official website, which will not be demonstrated here.

9090 (This is the port number for your VisualLogic project, which can be customized but should not conflict with the ports listed above)

After successfully opening the ports, the Baota panel can be accessed.

> Port configuration interface reference for Alibaba Cloud servers

> Port configuration interface reference for Tencent Cloud servers

## 2. Install Software

Upon the first opening of the Baota panel, it's typically prompted to pre-install software. Follow the prompts to install the regular LNMP stack.

Ensure the installation of the following software:

- Nginx
- MySQL (Version 5.7 or 8.0)
- Redis (Usually not included in the pre-installed software, please install it separately from the Baota panel's software store)

## 3. Configure Domain Name and WeChat Official Account

### Domain Name Resolution

Add a record that points to the server IP; for example, using h5 as a subdomain:

- Record type is A

### Site Creation

In the Baota panel, input the previously parsed subdomain and main domain, e.g., h5.xxx.com. Leave the rest as default, and click Submit.

### Install SSL Certificate

An SSL certificate must be installed to use HTTPS access. Otherwise, only HTTP can be used.

After installing the certificate, you can choose one of the two free SSL certificates provided by Baota.

At this point, entering the domain name h5.xxx.com in the browser should be accessible. If not, check domain name resolution; if HTTPS access fails, verify the SSL certificate configuration.

### WeChat Official Account Configuration

Open the WeChat Official Account backend, and configure the three domains marked in the red box by adding the previously configured h5.xxx.com to these domain settings. Initially, h5.xxx.com cannot be submitted.

Download the file marked in red to your computer, return to the Baota panel, upload the file to the root directory of h5.xxx.com, and submit on the official account page.

After setting up the three domain configurations for the official account, begin setting up the whitelist by adding the server IP to it, concluding the configuration for the WeChat official account.

For detailed configurations, refer to: [Configuration Reference Document](https://www.VisualLogic.cn/docview?page=r10136l173l194#)

### Set Up Reverse Proxy

Locate the site h5.xxx.com, click Settings, and enter the reverse proxy. Then input:

```http
http://127.0.0.1:<VisualLogic port number> e.g., http://127.0.0.1:9090
```

Click Save.

> Note: The sequence of setting official account configuration and reverse proxy should not be reversed.

### Open VisualLogic Port

In the Baota panel, click Security, input the VisualLogic project port number 9090 with remarks, and click Open.

## 4. Create Database

Access the database management in the Baota panel, click on the root password to copy or reset a root password, and then add a database:

- Database Name: h5 (customizable)
- Username: h5 (customizable)
- Password: Set as root password

With a standard Baota panel and MySQL, this concludes the database configuration.

## 5. Install VisualLogic Base

Enter the file section, open the root folder, create a new folder named VisualLogicTest, and then enter this folder. Upload the base file and the exported application deployment package.

Enter the terminal, to ensure path consistency, input the following commands:

```shell
cd /
cd /root/VisualLogicTest
```

Then input:

```shell
./VisualLogicBase
```

Configure the following parameters:

- VisualLogic project port number: 9090
- log level: just press Enter
- redis address: just press Enter
- redis password: input according to your setting or just press Enter
- database: h5, then press Enter

Configuration successful.

### Configure Service

Set VisualLogicBase as a service and configure it to restart automatically:

#### Method 1: Create via Terminal Commands

Access the terminal:

```shell
cd /etc/systemd/system
sudo vim VisualLogicTest.service
```

Input the following content:

```plaintext
[Unit]
Description=VisualLogic test
After=network.target redis.service mysql.service
[Service]
Type=simple
User=root
Group=root
Restart=on-failure
RestartSec=10
WorkingDirectory=/root/VisualLogicTest
ExecStart=/root/VisualLogicTest/VisualLogicBase
[Install]
WantedBy=multi-user.target
```

Save and exit.

#### Method 2: Create via Baota Files

Enter the system file folder, create a file named VisualLogicTest.service and edit, inputting the above content to save.

Execute the following commands:

```shell
sudo systemctl daemon-reload
sudo systemctl start VisualLogicTest
sudo systemctl enable VisualLogicTest
```

Service started successfully.

## Flowchart

```mermaid
graph TD
    A[Install Baota on Server]
    B[Open Server Ports]
    C[Install Software]
    D[Configure Domain and WeChat Official Account]
    E[Create Database]
    F[Install VisualLogic Base]
    G[Configure Service]
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
```

## 6. Common Issues and Solutions

### WeChat Related Configuration Issues

**No forwarding text when forwarding:**

- Turn off forced HTTPS, restart the Nginx software on the server
- Check if the reverse proxy is missing the following configuration:

```plaintext
proxy_set_header X-Forwarded-Proto $scheme;
```

Save and restart Nginx.
 # glTF model

### glTF Model

#### Introduction

glTF (GL Transmission Format) is a standard format for 3D content, designed with optimized data structures for real-time rendering in applications. 3D models can be ideally stored in glTF format. glTF has the following key features:
- Better suited for engines based on OpenGL;
- Smaller file size;
- Shorter rendering time;
- Supports 3D model geometry, materials, animations, scenes, cameras, and other information;
- Offers two export format extensions: .gltf and .glb.

glTF is a common format for 3D models that can be used to store and render 3D models.

#### Structure of glTF Files

A glTF file typically consists of two main parts:
- **.gltf file**: A JSON file containing metadata of the 3D model, such as scenes, nodes, meshes, materials, etc.
- **.bin file**: A binary file containing the actual data of the 3D model, such as vertices, normals, texture coordinates, etc.

#### Advantages of glTF Files

- **Small Size**: glTF files are generally smaller compared to other 3D model formats, making them easier to store and transfer.
- **Easy to Parse**: The format of glTF files is very simple, making them easy to parse and use.
- **Supported by Various Tools**: Many 3D software and tools support glTF format.

#### Applications of glTF Files

glTF files can be used for various purposes, such as:
- **Game Development**: For storing and rendering 3D models in games.
- **Web Development**: To display 3D models on web pages.
- **Virtual Reality**: To create virtual reality experiences.
- **Augmented Reality**: To create augmented reality experiences.

#### Format of glTF Files

The glTF file format is JSON-based, defining the structure and data of 3D models.

#### Creating glTF Files

glTF files can be created using various tools, such as:
- **3D Modeling Software**: For instance, Blender, Maya, 3ds Max, etc.
- **Online Tools**: Like glTF Converter, etc.
- **Programming Languages**: Such as Python, JavaScript, etc.

#### Future of glTF Files

The glTF format is constantly evolving and is becoming the standard format for 3D models. As 3D technology progresses, glTF files will be used in more fields.

#### Basic Usage

##### Step 1: Create and Export a glTF Model

Using 3D software like Blender as an example, after creating the model in the software, export a glTF model that contains geometry, materials, lighting, and animations. Select the necessary data, with the example format being .glb.

```mermaid
graph TD
    A[Create Model] --> B[Choose Export Format]
    B --> C[Export .glb File]
```

After exporting the model file, you can upload it to a three.js editor ([three.js Editor](http://www.webgl3d.cn/threejs/editor/)) to preview the model's effects and verify that all necessary content has been correctly exported.

##### Step 2: Import the Model into the VisualLogic 3D World Component

Import the model file into the VisualLogic 3D world component. Click [Upload gltf Model] in the "3D World" component and choose the .glb file exported in the previous step. Note: If the previous step was exported in .gltf format, you need to package the exported files into a zip file before uploading.

```mermaid
graph TD
    A[Export File] --> B[Upload to three.js Editor]
    B --> C[Check Model Effects]
    C --> D[Upload to VisualLogic 3D World Component]
```

##### Step 3: Adjust and Control the Model

After uploading, you can make secondary adjustments to the model, such as:
- **Adjusting Material of Geometry**: This switch is off by default. Turning it on will override the original material settings.
- **Adjusting Lighting**: This switch is on by default. Turning it on uses the original file's lighting; turning it off allows lighting adjustments in the VisualLogic editor.
- **Manipulating Animations**:
- **Gesture Control**: Allows moving, rotating, and scaling of one or multiple models using gestures.

```mermaid
graph TD
    A[Adjust Model] --> B[Adjust Material]
    A --> C[Adjust Lighting]
    A --> D[Manipulate Animations]
    D --> E[Gesture Control]
```

For example, set the following properties to control model rotation with gestures (positive or negative sensitivity controls the rotation direction):

If you need to control only a subset of models, you can use the "Disable Gesture Control" switch to specify which models can be manipulated. For example, in the case below, when touching one model, the "Gesture Control" for another model is disabled.

Additional: [DEMO Download](https://www.ivx.cn/ih5/app/template/download?id=4285)"
 # HDR Ambient Light

### Introduction

HDR, short for High Dynamic Range, refers to an expanded range of light and color intensity to enhance visual effects. HDR environmental lighting is primarily used in 3D scenes, serving functions such as:

1. Acting as a background environment, for example, simulating a panoramic scene of blue skies and white clouds.
2. Serving as illumination and reflection sources for models, for example, simulating natural lighting effects on a model within an indoor environment.

### Basic Usage

#### 1. Acquiring HDR Files

Besides downloading .hdr files directly from the internet, you can also use Photoshop to convert standard panoramic images into .hdr format.

1. Prepare a standard panoramic image in a normal image format (with a left-to-right seamless connection and a 2:1 aspect ratio). Higher pixel count results in better quality, e.g.:
2. Open the panoramic image in Photoshop, set the "Image Mode" to 32 bits, and then "Save As" in "Radiance" format to obtain an .hdr file.

#### 2. Uploading HDR Environmental Lighting

1. Models appear black when there's no lighting.
2. In the VisualLogic editor's "3D World" component property panel, click the button to select and upload the .hdr file.
3. After a successful upload, both background and lighting will be applied.
4. You can choose the type of environmental lighting effect according to your needs.

### Attachments

[DEMO Download](https://www.ivx.cn/ih5/app/template/download?id=4291)

### Workflow Diagram

```mermaid
graph TD
    A[Acquire HDR File] --> B[Prepare Standard Panoramic Image]
    B --> C[Open Photoshop]
    C --> D[Set Image Mode to 32 bits]
    D --> E[Save as Radiance Format]
    E --> F[Obtain .hdr File]
    F --> G[Upload File in VisualLogic Editor]
    G --> H[Select HDR Environmental Light File]
    H --> I[Activate upon Successful Upload]
```"
 # Integrating External Systems & Resources

### # Overview

In today's complex development environment, the flexibility to integrate with various external systems and development resources is one of the key factors for ensuring the success of an application. This article provides a detailed introduction on how to achieve integration with external systems through VisualLogic tools, including custom functions, custom SDKs, API and public services, as well as DBO custom databases, helping developers to work quickly and effectively.

### # 1. Custom Functions

Custom functions are used to directly execute JavaScript code to achieve specific functionalities. Here are the steps to implement custom functions:

- Add a custom function component in the object tree.
- Define the function logic, and call the custom function via a service component on the VisualLogic backend to execute.

```mermaid
graph TD;
    A[Add custom function component in the object tree] --> B[Define function logic]
    B --> C[Backend service component calls custom function]
```

#### 2. Custom SDK

Custom SDK allows developers to use more complex code for backend operations. VisualLogic supports SDKs on Node.js and Java platforms:

- Create a Docker image and upload the code.
- Reference the corresponding SDK package in the object tree.
- Directly call the methods in the SDK in the service, similar to using custom functions.

```mermaid
graph TD;
    A[Create Docker image and upload code] --> B[Reference SDK in the object tree]
    B --> C[Call SDK methods in the service]
```

#### 3. API and Public Services

VisualLogic calls third-party services through HTTP interfaces and can also set services as public for external use:

- Call third-party APIs via backend API components.
- Set the service as public to be called through HTTP requests.

```mermaid
graph TD;
    A[Call backend API component] --> B[Set service as public]
    B --> C[Call service through HTTP request]
```

#### 4. DBO Custom Database

DBO components support connection to third-party database operations, and VisualLogic supports various relational databases (such as MySQL, PostgreSQL, SQL Server, and Oracle):

- Use DBO components to connect to the database.
- Execute database operations via SQL Query.
- Ensure the database allows access from the VisualLogic server and opens IP whitelist.

```mermaid
graph TD;
    A[DBO components connect to the database] --> B[Execute operations via SQL Query]
    B --> C[Database allows VisualLogic server access]
```

Through these methods, developers can easily and seamlessly connect and interact with various external systems using VisualLogic, thereby achieving rich functional components and accelerating the development process. This powerful tool from VisualLogic not only simplifies the complex integration process but also provides developers with flexible development options to assist in the successful implementation of projects.
 # Private Deployment Log Viewing

### View Private Deployment Logs via Influx CLI

#### Step 1: Install InfluxDB

To install InfluxDB on a local server, perform the following actions:

1. Navigate to the appropriate directory:
   ```shell
   cd /etc/yum.repos.d/
   ```
2. Create a new repo file:
   ```shell
   vim influxdb.repo
   ```
3. Copy and paste the following content into the new window, then save (press `i` to switch to insert mode, paste the text, press `ESC` and type `:wq` to save):
   ```plaintext
   [influxdb]
   name = InfluxDB Repository - RHEL $releasever
   baseurl = https://repos.influxdata.com/rhel/7/$basearch/stable
   enabled = 1
   gpgcheck = 1
   gpgkey = https://repos.influxdata.com/influxdata-archive_compat.key
   ```
4. Enter the following command to install InfluxDB:
   ```shell
   sudo yum install influxdb2
   ```
5. Start InfluxDB and let it run in the background, occupying port 8086:
   ```shell
   sudo systemctl start influxdb
   ```
6. Check the InfluxDB running status:
   ```shell
   sudo service influxdb status
   ```

   If you see "active running", the installation and startup are successful.

#### Step 2: Install Influx CLI

Influx CLI is a command-line tool for executing InfluxDB queries. The installation steps are as follows:

1. Open the Influx CLI website and download the appropriate version: [Influx CLI Official Download Page](https://docs.influxdata.com/influxdb/v2.1/tools/influx-cli/?t=Linux)
2. Switch the server to the root directory:

   ```shell
   cd /root
   ```
3. Download the installation tarball of Influx CLI (using the amd64 version as an example):

   ```shell
   wget https://dl.influxdata.com/influxdb/releases/influxdb2-client-2.2.0-linux-amd64.tar.gz
   ```
4. Extract the tarball:

   ```shell
   tar xvzf influxdb2-client-2.2.0-linux-amd64.tar.gz
   ```
5. Copy the `influx` file to the `/usr/bin/` directory:

   ```shell
   cp /root/influxdb2-client-2.2.0-linux-amd64/influx /usr/bin/
   ```
6. Make the file executable:

   ```shell
   chmod 777 /usr/bin/influx
   ```
7. Execute the following command to verify the installation:

   ```shell
   influx
   ```

   Seeing the relevant interface indicates a successful installation.

#### Step 3: Initialize InfluxDB with Influx CLI

1. Initialize InfluxDB:

   ```shell
   influx setup
   ```

   Enter the following information sequentially:

   - username: root
   - password: any content
   - organization name: VisualLogic (customizable, remember for later use)
   - bucket name: publish (must be publish)
   - retention period (bucket retention time, enter 0 for unlimited): 0
2. Retrieve and record the token:

   ```shell
   influx auth list -c default
   ```
3. Switch back to the directory where the VisualLogic base is located and modify the `config.json` file to add log configuration:

   ```json
   "log": {
       "address": "http://127.0.0.1:8086",
       "org": "Recorded organization name",
       "bucket": "publish",
       "token": "Recorded token"
   }
   ```

> **Note**: Before exporting the private deployment package, make sure to turn on the log switch in the background, otherwise, there will be no log records.

### Use Influx Query Command to Query Logs

The Influx Query command is used to query log records. You can refer to the following example command to query backend service logs for the last hour:

```shell
influx query 'from(bucket:"publish") |> range(start: -1h)'
```

For detailed usage of the `influx query` command, please refer to the official documentation: [Influx Query Documentation](https://docs.influxdata.com/influxdb/v2.1/query-data/get-started/query-influxdb/).

### Flowchart

```mermaid
graph TD
A[Install InfluxDB] --> B[Install Influx CLI]
B --> C[Initialize InfluxDB Settings]
C --> D[Modify Configuration File]
D --> E[Query Logs]
```"
 # Common issues in DBO

### # VisualLogic DBO Connection Overview

In VisualLogic, DBO connections are established through requests initiated by the VisualLogic backend server. Therefore, your database must be accessible via the public internet to connect normally in the editor. If you have a whitelist restriction, you need to add the VisualLogic server backend outbound IP: 52.83.157.21 and 52.83.192.50.

If the database cannot be accessed on the public internet, you can choose one of the following two methods:

1. Forego direct connection in the editor, configure SQL statements first, and then privately deploy the application to the same network environment as the database.
2. Use a privately deployed editor system to deploy the entire VisualLogic editing environment locally to achieve database connection.

### # Local Database Connection Method

When you try to connect the VisualLogic’s DBO to a local database, you might encounter connection issues due to the requirement for the database to be publicly accessible. To resolve this issue, you can:

- Forego direct configuration in the editor, complete the SQL statement configuration, and then privately deploy the application to the same network environment as the database.
- Choose a privately deployed editor system to deploy the entire VisualLogic editing environment locally.

Additionally, if a "connection reset by peer" error occurs when connecting to the database, it means the target database rejected the connection from the VisualLogic server. In this case, you can try using a custom DSN, adding extra connection parameters, or checking the database's connection restrictions. It is important to note that the DBO component, being a backend development tool, does not automatically add extra parameters, which may cause some databases to connect in tools like Navicat but fail with DBO.

### # Common Issues and Solutions

1. **"Connection reset by peer" Error when Connecting to Database**

   Solution: Add the `disable security` parameter in the custom DSN, such as `sqlserver://xx:xxx@111.21.151.61?database=master&encrypt=disable`. Different databases have their own parameter requirements, which can be found in the respective database connection documentation.

### # Precautions for Private Deployment

During the private deployment process, you might encounter issues with ports being occupied. For example, if running `./ivxbase` prompts “bind: address already in use,” it might be because a previously set backend service is still running. Solution:

- Check and close the process occupying the port.
- For instance, you can use `netstat -ntulp | grep 80` to find the process occupying port 80 and then use the `kill -9 pid` command to kill that process before continuing with `./ivxbase`.

Here are the main steps to connect to the database:

```mermaid
graph TB
    A[Start] --> B{Can the database be accessed publicly?}
    B -- Yes --> C[VisualLogic backend initiates database connection request]
    B -- No --> D{Choose connection method}
    D -- Method 1 --> E[Configure SQL statements and privately deploy]
    D -- Method 2 --> F[Use privately deployed editor system]
    G[End]
    C --> G
    E --> G
    F --> G
```

VisualLogic, in providing DBO connection services, enhances development efficiency with its code-free convenience and flexible component structure. By employing a reasonable connection strategy, users can not only achieve smooth database connections but also fully utilize the powerful features provided by VisualLogic to build stable and efficient systems.
 # Common Issues in Private Deployment

### Running ./VisualLogicBase Port Occupied, Prompt bind: address already in use

As shown below, the port in json.config is occupied (it may be due to a previously set background service VisualLogicTest running).

**Solution**:

1. **Case One**: If the background service `sudo systemctl start VisualLogicTest` was executed before, you can first execute `sudo systemctl stop VisualLogicTest` to stop the service and manually execute `VisualLogicBase`.
2. **Case Two**: If it's unclear which application (process) is occupying the port, you can kill the application. For example, if port 80 is occupied, use the command:

```shell
netstat -ntulp | grep 80  // Find the PID process occupying port 80
```

After finding the process PID that occupies port 80, execute:

```shell
kill -9 7371  // 7371 is the process PID
```

This will kill the process occupying the port and continue executing `./VisualLogicBase`.

```mermaid
graph LR
A[Start VisualLogicBase] --> B{Is the port occupied?}
B -- Yes --> C{Executed VisualLogicTest before?}
C -- Yes --> D[Execute command to stop VisualLogicTest]
C -- No --> E[Find occupying process via command]
E --> F[Kill the process]
B -- No --> G[Continue executing ./VisualLogicBase]
D --> G
F --> G
```

### After Private Deployment, File Interface Returns Undefined for Video/Audio Duration, Cover, Resolution

After private deployment, attributes like total video/audio duration, cover, resolution need to be generated by Linux using ffmpeg. If the user's server doesn't have this program installed, it cannot be generated.

**Solution**:

#### Linux Server:

1. Download and install the package.
   [Download link](https://file3.ih5.cn/v35/files/ffmpeg.tar.xz)
2. Extract the file.
3. Place the extracted contents in the server's `/usr/bin` directory.
4. Navigate to the `/usr/bin` directory and make `ffmpeg` and `ffprobe` executable:

```shell
chmod +x ffmpeg ffprobe
```

5. Return to the deployed application and perform the "upload video" action to check return results.

#### Windows Server:

1. Download the Windows version package of ffmpeg.
   [Download link](https://file3.ih5.cn/v35/files/2e842cff9beb76728519066571970f35_85247487.zip)
2. Extract the downloaded ffmpeg to a specified directory.
3. Right-click "This PC" → "Properties" → "Advanced System Settings" → "Environment Variables". Add the extraction path to the `path` variable in system variables. E.g., `F:\ffmpeg-4.0-win64-static\ffmpeg-4.0-win64-static\bin`
4. After configuration, run cmd and enter `ffmpeg -version`, if version information is displayed, the configuration is successful.

```mermaid
graph TD
A[Install ffmpeg] --> B{Is ffmpeg installed?}
B -- Yes --> C[Configure environment variables]
B -- No --> D[Download ffmpeg package]
D --> E[Extract to specified directory]
E --> C
C --> F[Run cmd to verify]
F --> G[Private deployment complete]
```

### After Private Deployment, Backend Database and File Interface Still Use VisualLogic Services

If the Android apk and Windows .exe are not handled before export, the backend part will still use VisualLogic services.

**Solution**:

1. Export the case as h5 to the server and redeploy.
2. Follow the tutorial for handling.

### After Private Deployment, WeChat Open Error 10003 redirect_uri Error

**Solution**:

Please refer to the precautions for WeChat interface integration during private deployment.

### After Private Deployment on Windows Server, nginx as Proxy Server Causes Some WeChat Interface Failures

**Problem Description**:

In some instances, after private deployment on Windows servers, using nginx as a proxy server may lead to some WeChat interface failures (e.g., obtaining phone numbers in private deployed applets, or obtaining current user info from WeChat official account).

**Solution**:

It is recommended to refer to detailed tutorials and adjust nginx configurations according to specific needs.

### Email Sending Failure After Private Deployment

**Problem Description**:

Email sending failure may stem from configuration issues after private deployment.

**Solution**:

Please refer to the detailed tutorial for the corresponding issue.

### SMS Sending Failure After Private Deployment

**Problem Description**:

When using the "SMS" component for custom SMS, Alibaba Cloud's SMS service needs to be configured.

**Solution**:

1. To use Alibaba Cloud's SMS service, configure the `sms` field in `config.json`.
2. Set the provider to `aliyun`.
3. Obtain and set `accessKeyId` and `accessKeySecret`.
4. Configure the `templateConfig` field in the format: `any name: your signature, your template Code`.

```mermaid
graph TD
A[Post-deployment Configuration] --> B{SMS sending failure}
B -- Yes --> C[Configure Alibaba Cloud SMS service]
C --> D[Obtain accessKeyId and accessKeySecret]
D --> E[Set templateConfig]
E --> F[Resend SMS]
B -- No --> G[Configuration successful]
G --> F
```"
 # Configure Reverse Proxy: Caddy

## # Introduction to Caddy

Caddy is a powerful, enterprise-grade, open-source web server written in Go, capable of automatic HTTPS encryption (i.e., automatic TLS certificate configuration and management). Similar to Nginx, Caddy can help host web services and allow others to access them via the internet. Compared to Nginx, Caddy's configuration is simpler and more straightforward. Since it is written in Go, Caddy is a standalone binary, requiring no dependencies and can be downloaded and installed directly from the official website.

## # Configuring Caddy in a Linux Environment

### # Download and Installation

1. First, open your browser and visit the [Caddy official website](https://caddyserver.com/download). Choose the appropriate binary package for your operating system (select the amd64 binary package for X86 64-bit systems).
2. After downloading, place the file in a directory on your disk, such as the C drive.
3. Use the `scp` command to upload the file to your server. In the example, the file is uploaded to the `root/testA` directory:
4. Use the `chmod` command to make the file executable:
```
   chmod +x caddy_linux_amd64
   ```
5. Move the file to `/usr/local/bin` and name it `caddy`:
   ```
   mv caddy_linux_amd64 /usr/local/bin/caddy
   ```
6. The installation is now complete. Verify the installation by checking the version information with the following command:
   ```
   caddy version
   ```

### # Creating and Starting the Caddy Service

1. Create the service file:
   ```
   vim /etc/systemd/system/caddy.service
   ```
2. On the newly opened blank screen, press the insert key and then copy and paste the following content:
   ```
   # caddy.service
   #
   # For using Caddy with a config file.
   #
   # Make sure the ExecStart and ExecReload commands are correct
   # for your installation.
   #
   # See https://caddyserver.com/docs/install for instructions.
   #
   # WARNING: This service does not use the --resume flag, so if you
   # use the API to make changes, they will be overwritten by the
   # Caddyfile next time the service is restarted. If you intend to
   # use Caddy's API to configure it, add the --resume flag to the
   # `caddy run` command or use the caddy-api.service file instead.

   [Unit]
   Description=Caddy
   Documentation=https://caddyserver.com/docs/
   After=network.target network-online.target
   Requires=network-online.target

   [Service]
   User=root
   Group=root
   ExecStart=/usr/local/bin/caddy run --environ --config /etc/caddy/Caddyfile
   ExecReload=/usr/local/bin/caddy reload --config /etc/caddy/Caddyfile
   TimeoutStopSec=5s
   LimitNOFILE=1048576
   LimitNPROC=512
   PrivateTmp=true
   ProtectSystem=full
   AmbientCapabilities=CAP_NET_BIND_SERVICE

   [Install]
   WantedBy=multi-user.target
   ```
3. Save the file and enter the following commands:
   ```
   systemctl daemon-reload 
   systemctl enable caddy
   ```

## # Configuring the Caddyfile

1. Create the directory and the configuration file:
   ```
   mkdir /etc/caddy
   vim /etc/caddy/Caddyfile
   ```
2. On the newly opened blank screen, press the insert key, then copy and paste the following content:
   ```
   www.example.com {
     tls xxxx@email.com
     reverse_proxy * http://127.0.0.1:9900
   }
   ```

   Note: Replace the domain (www.example.com), email (xxxx@email.com), and port (9900) with actual information.
3. Save the file and start the Caddy service:
   ```
   systemctl start caddy
   ```

## # Implementing Automatic HTTPS for Arbitrary Domains with Caddy

Below is the method to achieve automatic HTTPS for arbitrary domains, with the Caddyfile content as follows:

   ```
:80, :443 {
  tls xxxx@email.com {
    on_demand
  }
  reverse_proxy * http://127.0.0.1:9900
}
```

## # Deploying Caddy Using Docker

If you are familiar with Docker, you can deploy using Docker by following these steps:

1. Refer to the Caddy official Docker documentation ([Caddy Docker Hub](https://hub.docker.com/_/caddy)).
2. Regardless of the method, the content of the Caddyfile remains the same as above.

## # Configuring Caddy on Windows

1. Download the appropriate version of the Caddy program and place it in any location, renaming it to `caddy.exe`.
2. Create and edit the configuration file as follows:
   - Create a new .txt text document and name it `Caddyfile.txt`.
   - Open the file with Notepad, copy the contents of the Caddyfile and save it.
   - Remove the .txt extension.
3. Run the Caddy program:
   - Directly double-click `caddy.exe` to run it, if ineffective, use the command line:
   - Open the Run panel (Win+R), enter `cmd` to enter the command line.
   - Navigate to the directory containing `caddy.exe`, and run the command:
```
     cd C:\caddy\
     caddy.exe run
     ```

#### Deployment Flowchart

```mermaid
graph LR
A[Download Caddy] --> B[Upload to Server]
B --> C[Grant Execution Permission]
C --> D[Move and Rename]
D --> E[Create Service File]
E --> F[Start and Enable Service]
F --> G[Create Caddyfile]
G --> H[Start Caddy Service]
```

### Reverse Proxy Solution Recommendation

When choosing a reverse proxy solution, the following recommendations are given:

- If the server is a virtual machine purchased from a cloud service provider or requires cluster deployment or Docker container deployment, it is recommended to prioritize the load balancing service provided by the cloud service provider. It is more stable, easier to configure, and supports clusters.
- If using a single machine or looking to save costs by deploying a web server yourself, it is recommended to use Caddy. Compared to traditional Apache and Nginx, Caddy is more lightweight, efficient, supports automatic HTTPS certificate generation, and is simpler to configure.
 # Detailed Analysis of Complexity

## Functional Overview and Definition of Complexity

Complexity is a concept pioneered by the VisualLogic platform, serving as a quantitative score for the scale and intricacy of applications developed using the VisualLogic platform. Generally, the higher the complexity of an application, the more complex and larger the scale, which also indicates a longer development cycle.

Within VisualLogic, the basis for measuring complexity includes the number of objects in the object tree and the number of valid lines in the event panel. The sum of these two metrics constitutes the complexity score of the application.

Specifically:

- Each valid edit line in the event panel is counted as 1 point;
- Each additional object in the object tree is counted as 1 point.

The resulting formula is:
Application Complexity = Number of objects in the object tree + Number of valid lines in the event panel

For example, if there are 19 lines of editable content in the event panel and several objects in the object tree, the complexity of the application would be 19 points.

## Concept of Historical Complexity

Considering that users frequently add and remove event lines and objects during long-term editing and maintenance of an application, the concept of "Historical Complexity" was introduced to more accurately reflect the cumulative complexity of application development.

Historical Complexity is an ever-increasing metric. If a user deletes objects or events, the complexity does not decrease; if a user adds objects or events, the complexity increases. Additionally, if only the properties of objects or configurations of events are changed without any additions or deletions, the complexity remains unchanged.

Therefore, Historical Complexity takes into account the user's usage path and better reflects the actual workload during the application development process. For example, if a user deletes objects or events, Historical Complexity does not decrease; but as soon as objects or events are added, Historical Complexity increases accordingly.

This "summation of history" approach ensures that complexity can effectively track the evolution and development history of the application. Users can use this quantitative method to better understand and manage their development workload and application complexity, thereby reasonably allocating development resources and time.
 # Standalone Deployment Three: Application Update and Multi-Application Deployment

#### Application Update

In a standalone deployment, application updates are a critical step. When using the VisualLogic approach, we can easily complete application code updates. The specific steps are as follows:

1. **Re-export the application package**: First, re-export the latest application package and upload it to the directory where VisualLogic Base is located.
2. **Configure the `config` file**: In the `config` file, there is a `loadPackage--period` option that sets the interval at which VisualLogic Base checks for application updates. By default, this interval is 1 second.
   
   - **Set the update check duration**: If the application files are stored locally, the default duration of 1 second is appropriate. However, if application files are stored in third-party storage services like OSS (e.g., Alibaba Cloud Object Storage Service), it is recommended to set this duration to a longer time (like 99999 seconds) to reduce access frequency and lower network costs. Adjust this duration to a shorter value when needing to update the application, and revert to a longer duration once the update is complete.
3. **Only recommend updating code**: It is recommended to update only the code rather than overwriting data files. This is because overwriting data on a running application can lead to the use of preview data on the current application, thus overwriting existing data in the private deployment database.

```mermaid
graph TD
    A[Re-export Application Package] --> B[Upload to VisualLogic Base Directory]
    B --> C[Configure Config File]
    C --> D{File Storage Location}
    D -->|Local| E[Default 1 Second]
    D -->|OSS or Other Third-party Storage| F[Set to 99999 Seconds]
    F --> G[Decrease Duration When Update Needed]
    G --> H[Revert to Longer Duration After Update]
```

#### Multi-Application Deployment

In the VisualLogic system, deploying multiple applications is also very convenient. The specific operational steps are as follows:

1. **Export a new case package**: First, upload the new case export package to the directory where VisualLogic Base is located on the server.
2. **Modify the `config` file**: After the first run of VisualLogic Base, a `config.json` file will be generated in the same directory. You can directly modify this configuration file to add new applications.
   
   - **Enter vim mode to edit the configuration file**: Enter the following command in the terminal to open vim mode:
     ```
     vim config.json
     ```
   - **Add new cases**: In vim edit mode, you can add new case nids in the `nids` array. Save and exit the vim editor once complete.
3. **Application Deployment Effective**: If the VisualLogic Base program is running, the just uploaded zip file will automatically disappear, indicating successful deployment of the new case.
4. **Database Reference Relationship**: The reference relationship between the application database and the account database will remain unchanged. For example, if application A and application B both use the same account database, they will continue to share it after export.
5. **Microservices not yet supported in private deployments**: Currently, the microservices feature does not support private deployments.

```mermaid
graph TD
    I[Export New Case Package] --> J[Upload to VisualLogic Base Directory]
    J --> K[Modify Config File]
    K --> L[Enter Vim Mode to Edit]
    L --> M[Add New Case Nid in Nids Array]
    M --> N[Save and Exit]
    N --> O[Automatically Disappear]
    O --> P[New Case Successfully Deployed]
```

By following these steps, using the VisualLogic tool can easily achieve application updates and multi-application deployment in a standalone deployment environment. This method simplifies complex operations, making the deployment process more efficient, saving development and maintenance costs, and provides developers with a flexible and convenient solution.
 # Configure Reverse Proxy: Nginx

## Configure Reverse Proxy and HTTPS Access

In the previous section on domain configuration, we enabled application access with domain plus port, such as: http://VisualLogictest.ih5school.cn:9900/Q&A. Next, to remove this port “:9900” from the access path and enable HTTPS access, we need to rely on a reverse proxy program. A reverse proxy is an intermediary program between user requests and backend services. Reverse proxy services can forward requests to a specified internal server address based on the user's domain and path and provide HTTPS certificate services. Additionally, when there are multiple backend services (in a clustered deployment), it performs load balancing. Currently, common reverse proxy services are typically integrated into web services programs, including:

- Nginx
- Apache
- Caddy
- IIS (Windows servers only)

These programs can be installed directly on the local server or on a separate server/container. Cloud service providers like Alibaba Cloud, Tencent Cloud, etc., offer load balancing services, which can be understood as pre-configured reverse proxy services on cloud servers.

## Configuring Nginx on Linux

First, log into your server and switch to the root user with the following command:

```bash
sudo su
```

Then execute two installation commands:

```bash
yum install nginx
yum install python3-certbot-nginx
```

Note: The second package is based on Python3. If your server doesn't currently have Python, you'll need to install it first, or the installation will fail.

After the two programs are installed, you can start the second automatic certificate service to automatically obtain a certificate:

```bash
certbot --nginx -d VisualLogictest.ih5school.cn
```

The first time you run this service, you'll need to enter an email address. Enter your email to receive notifications before the certificate expires. After accepting the terms of service and entering the email, the certificate will be generated automatically. When you see the "congratulations" message, it indicates that the certificate has been successfully generated.

Note: Each certificate has an expiration date. This free certificate expires in about 3 months, so you'll need to regenerate the certificate before it expires by using:

```bash
certbot renew
```

The above certificate is a Let's Encrypt certificate, the largest free certificate platform worldwide, known for excellent browser compatibility and security. VisualLogic's public platform currently uses this free certificate, so you can use it with confidence.

Finally, we need to configure the nginx configuration file. In root mode, open the nginx configuration file with the vim editor:

```bash
vim /etc/nginx/nginx.conf
```

At the end of the file, find the configuration related to port 443:

```
server {
    listen 443 ssl;
    server_name VisualLogictest.ih5school.cn;
    ssl_certificate /etc/letsencrypt/live/VisualLogictest.ih5school.cn/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/VisualLogictest.ih5school.cn/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:9900;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

In the above configuration, you only need to change the port in the first line to your corresponding service port, and the rest can be copied directly. These settings are primarily for services like WeChat authorization to work properly, ensuring proper request handling by nginx.

After entering, the config file looks like this:

```
server {
    listen 443 ssl;
    server_name VisualLogictest.ih5school.cn;
    ssl_certificate /etc/letsencrypt/live/VisualLogictest.ih5school.cn/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/VisualLogictest.ih5school.cn/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:9900;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Finish editing and restart nginx with the following commands:

```bash
1. Kill previous nginx process: killall nginx
2. Restart nginx service: systemctl restart nginx
```

Now that we've configured the reverse proxy, you can access our previous example directly through an HTTPS domain without using a port:
http://VisualLogictest.ih5school.cn:9900/Q&A ----》https://VisualLogictest.ih5school.cn/Q&A

```mermaid
graph TD;
    A[Login to Server] --> B[Switch to Root User];
    B --> C[Install Nginx];
    C --> D[Install Certbot];
    D --> E[Obtain Certificate];
    E --> F[Configure Nginx File];
    F --> G[Restart Nginx Service];
    G --> H[Access Application via HTTPS];
```

## Configuring Nginx on Windows

1. Visit the nginx official website, download the latest Windows version of nginx.

Download the zip package, extract it to a directory, and double-click to run nginx.exe. Once successful, you can see the nginx.exe process in the Windows Server task manager.

Certificate Configuration

You'll need to prepare two certificate-related files matching the domain. If you don’t have certificate files, you can apply for a free certificate for your domain from Tencent Cloud, then select the server type as Nginx for the certificate file package to download and extract the .crt and .key files into nginx's conf directory.

2. Enter the nginx directory, open the nginx.conf file, and find the content beneath #server. Modify, save (modification details below):

```
server {
    listen 443 ssl;
    server_name wintest.vxplo.cn;
    ssl_certificate wintest.vxplo.cn_bundle.crt;
    ssl_certificate_key wintest.vxplo.cn.key;

    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://127.0.0.1:9900;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

3. Restart nginx

After every modification of the nginx.conf file, you need to restart nginx.exe for the changes to take effect. Open the Windows Server task manager, find the two nginx.exe processes, right-click to end them, and then double-click to run nginx.exe again to restart and apply the certificate configurations from step 2.

Finally, you can access the VisualLogic application on the Windows server using an HTTPS domain plus path like this: https://wintest.vxplo.cn/play/SVXaccNH.

```mermaid
graph TD;
    A[Download and Extract Nginx] --> B[Run nginx.exe];
    B --> C[Open nginx.conf File];
    C --> D[Configure Certificate];
    D --> E[Modify Configuration File];
    E --> F[Restart nginx.exe];
    F --> G[Access Application via HTTPS];
```

ps: The following content is not mandatory
If there are other services already occupied/matched on the server, you can write an additional location for more precise matching to differentiate.

```
location ~ ^/(play|ih5|api)/ {
    proxy_pass http://127.0.0.1:9900;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

This matches paths starting with `/play/` or `/ih5/` or `/api/` to be forwarded for processing. `/play/...` is the default path for accessing application pages, `/ih5/...` is for accessing application resources including socket connections, and `/api/...` is for accessing public services of the application.

## Recommended Reverse Proxy Solutions

Among the above reverse proxy solutions, the following are recommended:

If you're using a server purchased from a cloud service provider like a virtual machine or require cluster deployment or docker container deployment, it's preferred to choose the load balancing services provided by cloud service providers. These are more stable, easier to configure, and support clustering. For more details, you can refer to this Alibaba Cloud's load balancing configuration tutorial.

If using a single machine or aiming to save costs by self-deploying a web server, it's recommended to use Caddy, which is lighter, more efficient, and easier to configure compared to traditional Apache and Nginx, and it supports automatic HTTPS certificate generation.
 # Reverse Proxy and HTTPS Access

### Overview

In the previous section on domain configuration, we implemented application access using domain names with ports, such as:

```
http://VisualLogictest.ih5school.cn:9900/Q&A
```

Next, to remove the port “:9900” from the access path and implement HTTPS access, we need to rely on a reverse proxy program. A reverse proxy is an intermediary program between user requests and backend service programs:

```mermaid
graph TD
    A[User Request] --> B{Reverse Proxy Service}
    B --> C[Backend Application]
    B --> D[Backend Application]
    B --> E[Backend Application]
```

A reverse proxy service can forward requests to a specified internal server address based on the user's domain name and path, and provide HTTPS certificate services. Additionally, when there are multiple backend services (in a clustered deployment), it performs load balancing.

### Common Reverse Proxy Services

Currently, commonly used reverse proxy services are usually integrated into web service programs, with the following being typical:

- Nginx
- Apache
- Caddy
- IIS (Windows servers only)

These programs can be directly installed locally on a server, or on another separate server/container.

Moreover, major cloud service providers like Alibaba Cloud, Tencent Cloud, etc., offer load balancing services. Load balancing can be understood as a pre-configured reverse proxy service deployed on cloud servers.

### What is a Reverse Proxy

A reverse proxy service can forward requests to a specified internal server address based on the user's domain name and path, providing HTTPS certificate services. Additionally, when there are multiple backend service programs (in a clustered deployment), it performs load balancing.

Common reverse proxy services are typically integrated into web service programs, including:

- Nginx
- Apache
- Caddy
- IIS (Windows servers only)

These programs can be directly installed locally on a server, or on another separate server/container.

Furthermore, major cloud service providers like Alibaba Cloud, Tencent Cloud, etc., offer load balancing services. Load balancing can be viewed as a reverse proxy service configured on cloud servers.

### Recommended Reverse Proxy Solutions

Among the above reverse proxy solutions, the following are recommended:

1. **Use Cloud Provider's Load Balancing Service**

   - If you are using virtual machines purchased from a cloud provider or require clustered or Docker container deployments, prioritize using the load balancing service from cloud providers. It is more stable, easiest to configure, and supports clustering.
   - You can refer to Alibaba Cloud's load balancing configuration tutorial for details.
   
2. **Using a Self-Hosted Web Server**

   - If you are using a single machine or want to save costs by deploying your own web server, Caddy is suggested. Compared to traditional Apache and Nginx, Caddy is more lightweight, efficient, simpler to configure, and supports automatic generation of HTTPS certificates.

### Workflow Diagram

Below is a workflow diagram for implementing reverse proxy and HTTPS access:

```mermaid
graph TD
    Step1[User Request] --> Step2{Reverse Proxy Service}
    Step2 --> Step3[Parse User Request]
    Step3 --> Step4[Forward Request Based on Domain and Path]
    Step4 --> Step5[Specified Internal Server Address]
    Step3 --> Step6[Provide HTTPS Certificate Service]
    Step2 --> Step7[Load Balancing]
```

### Conclusion

By adopting a reverse proxy service, we can simplify the user access path, thereby enhancing the user experience. At the same time, the reverse proxy service provides HTTPS certificate services to ensure the security of data transmission. In the case of multiple backend services, reverse proxy services can also perform load balancing to improve system stability and performance. When choosing a specific reverse proxy solution, you can opt for a cloud provider's load balancing service or self-hosted web server based on actual needs. Either way, you can effectively enhance application access efficiency and security.
 # VisualLogic Development Specification

## Basic Principles

1. Program correctness.
2. Program maintainability without conflicting with point 1.
3. High operational efficiency without causing issues for other developers, without conflicting with points 1 and 2.

## I. Comprehensive

### Optimizing/Improving Application Performance

#### Use fx for Handling Large Temporary Data

If a developer has a programming background, it is not recommended to use VisualLogic-loop syntax when handling large datasets. Instead, use native JS loop statements within the fx component. Prefer native for loops over map/each API syntax. When handling arrays with a large sample size in VisualLogic, processing time scales proportionally: for loop:jsapi:VisualLogic-loop = 1:10:100. Consider VisualLogic-loop only when the data sample is small and does not require multi-layer loops.

#### Selectively Import Resources

Some external resources, such as external libraries and font files, can be large; for example, Chinese fonts may reach 10MB. VisualLogic's loading strategy is to render the application after all resources are loaded, which can block page generation. Minimize references to overly large external resources.

#### Reuse Variables/UI/Events

For instance, for a modal that only displays different logos or messages, use dynamic variables to control the displayed content. Events used in multiple places can be packaged into action groups for reuse.

### Improving Development Efficiency

#### Naming Added Components

Minimize the use of default variable or object names to enhance maintainability.

##### Recommended Naming Rules

Component names should align with their functionality and semantics, and be easy to search. For example:

| Example                       | Not Recommended | Better Naming     |
| ----------------------------- | --------------- | ----------------- |
| User Login Button             | User Login      | User Login Button |
| User Login Modal              | User Login      | User Login Modal  |
| Order List Query Action Group | Order List      | Order List Query  |

#### Use Static Variables for Syntax Judgments

Static variables don't change, such as `itemStatus` for product status, with a value of `{"Ordered":0,"Shipped":1}`. They enhance readability and maintainability during use.

#### Write Good Comments

VisualLogic comments are categorized into inline notes, interline notes, and group notes. Interline notes have better readability and can summarize or segment subsequent action logic.

#### Avoid Nested Callback Layers

Within the same event, embedded "callback" actions should not exceed 14 to prevent editor layout issues. It's recommended to distribute these "callback" actions across multiple action groups.

#### Simplify Operations in Data Binding

Data should be processed before binding to avoid complex function calls at the data binding site.

#### Avoid Mixing Data-Driven and Object-Driven Rendering Methods for Attribute Modifications

Attributes in components that change should be rendered through data binding to avoid setting properties directly in actions, preventing rendering conflicts.

#### Logical Grouping and Reusability

Logical units include small modules (reusable), functions (reusable), event groups (reusable), and action groups (non-reusable).

### Security Issues

#### APIs Should Be Called from the Backend Whenever Possible

#### Do Not Display Debug Content in Production Versions

Initialize domain checks and use `console.log=function(){}` to override debugging.

### Code Readability

#### Ternary Expressions, Maximum One Level

Complexly nested ternary expressions are discouraged. Consider using variable binding chains to handle logic, breaking down complex judgments into multiple meaningful variables for easier debugging.

## II. Frontend

### Page Layout Guidelines

#### Minimize the Use of Row and Column Components

Avoid using row and column components solely for distinction or placement.

#### Absolute Positioning & Relative Positioning

Choose optimization strategies appropriately: absolute positioning for performance, relative positioning for adaptive and complex layouts.

### Frontend Editing Guidelines

#### Avoid Loop Calling APIs/Services

If necessary, simulate loops using triggers.

### Attention to Frontend Behavior

#### Improve Refresh/Back Navigation Logic

Enhance application page routing and support for browser refresh and back button features.

#### Do Not Generate Critical Information on the Frontend

For instance, logic for red packets should be handled in the backend to prevent tampering.

#### Avoid Adding Multiple Canvases/3D Worlds in a Single Application

Use a single canvas within a page to conserve resources.

#### Use Form Containers

For functionalities involving more than three form-type components, use form containers for validation and aggregation.

### Frontend Object Tree Structure

#### Object Categorization

Variables and constants should be distinct, logic should be clear; action groups, variables, etc., should precede components.

## III. Backend

### Authentication Methods

#### Internal Projects

Use the userinfo interface uniformly to obtain current user login information.

#### External Projects

JWT TOKEN method is recommended to ensure security. Main steps include: the user inputs login credentials, the server validates and returns a signed token, the client stores the token, API requests include the token, and the server validates the JWT.

### Service/Parameter Naming

#### Service Naming

Include path content in service names for easier searchability.

#### Parameter Naming

Use camelCase for parameter naming, ensuring they are descriptive and try to use common internal system parameter names.

### Basic Parameter Explanation

#### Response Parameters

code denotes error code, typically 200 for success; reason explains the error reason; data carries returned data.

#### Request Parameters

pageIndex and limit for pagination; it’s recommended that unnecessary parameters are not returned.

### Database Documentation Specification

Include CRUD operations, field types, and descriptions.

### API Documentation Specification

Should cover CRUD operations and their formats of the service.

### Backend Attention to Behavior

Avoid API calls within transactions whenever possible; instead, use task queues. Do not perform database read/write operations in VisualLogic-loops.

### Backend Object Tree Structure

Place general services, action groups, and variables under the general module container; classify databases separately, and categorize by different business modules.
 # Deploying in Alibaba Cloud Container Service

## How to Deploy VisualLogic Applications on Alibaba Cloud Container Service

To run VisualLogic applications using Alibaba Cloud Container (Docker), we must prepare the following resources in advance:

- Object Storage (OSS/S3)
- Cloud Database RDS
- Cloud Database Redis

Since containers are stateless (i.e., they do not store application data) pure compute services, we need to connect shared file storage (Object Storage) and database services (Cloud Database RDS).

### I. Create a Cluster

Navigate to the "Cluster List" in Alibaba Cloud Container Service, or access the cluster through the "Elastic Container" left navigation bar under Serverless Kubernetes and click "Create Cluster" in the upper right corner.

1. Select "ASK Cluster" from the navigation bar and fill in the "Cluster Name";
2. Choose "Standard Edition" for cluster specifications;
3. Select the appropriate region (can increase user access speed by choosing the region close to the target users).
4. The other settings can remain default, then continue clicking "Next: Component Configuration";
5. Click "Next: Confirm Configuration";
6. Check "Service Agreement" and then click "Create Cluster."

Once the cluster is successfully created, its status will be "Running." Click on the cluster name to enter the configuration.

### II. Configure the Cluster

#### 1. Configuration Management

In configuration management, fill in the config.json information:

- The configuration item name can be arbitrarily named, such as visuallogic-config;
- Name can also be arbitrarily named, such as visuallogic-config-detail;
- The value must be in standard JSON format: check the external link, and replace the XXXX in the link with the actual information for Redis, object storage, and cloud database.

Once the configuration item is successfully created, the configuration item name visuallogic-config will appear.

#### 2. Create a Workload (Stateless Deployment)

- Fill in the application name and the number of replicas (number of instances that need to run concurrently, can be modified later);
- Fill in the image name, image tag, CPU memory configuration, and open ports according to the prompts (note that the name must start with http), the environment variable name must be CONFIG_JSON (VisualLogic will look for CONFIG_JSON by default to obtain relevant configuration information);
- After clicking next, click Service to create a Load Balancer, so it can be accessed via the public network;
- Note that the name in port mapping must start with http, the service ports are 80 and 443, all mapped to the container's 80 port.

After completion, you can see two VisualLogic bases in running status, indicating success. Through the logs, you can view execution records of VisualLogic, including the paths of running cases.

At this point, you can go to "Network" -> "Service" to find the IP under "External Endpoint," and access the case using IP address+/play/xxxx.

```mermaid
graph TD
    A[Enter Alibaba Cloud Container Service] --> B[Select Serverless Kubernetes]
    B --> C[Click Create Cluster]
    C --> D[Fill in Cluster Name and Specs]
    D --> E[Select Region]
    E --> F[Configure Steps]
    F --> G[Confirm and Create Cluster]
    G --> H[Fill in Configuration Items in Configuration Management]
    H --> I[Create Workload]
    I --> J[Fill in Info and Create Service]
    J --> K[Public Network Access to VisualLogic Base]
```

### Access Cases Using HTTPS Domain

1. Enter Alibaba Cloud Load Balancer (CLB) to create a traditional load balancer. Once created, a public IP will be assigned;
2. Go to your domain management and CNAME the domain to the public IP of the load balancer, and prepare an SSL certificate (nginx-compatible);
3. Upload the prepared SSL certificate (.pem and .key files) to the certificate management in the Alibaba Cloud Load Balancer;
4. Return to the traditional load balancer (CLB), click the listening configuration wizard, select HTTPS protocol, and listen on port 443;
5. Bind the previously uploaded SSL certificate and select the server group;
6. After clicking finish, you can access the case via the HTTPS domain + path.

To import more cases, fill in nid by following the steps below, and restart VisualLogic bases by scaling the container number (the exported case zip needs to be uploaded to the zip folder in OSS in advance).

```mermaid
graph TD
    A[Enter Alibaba Cloud Load Balancer] --> B[Create Traditional Load Balancer]
    B --> C[Assign Public IP]
    C --> D[CNAME Domain to Public IP]
    D --> E[Prepare SSL Certificate]
    E --> F[Upload SSL Certificate to Load Balancer]
    F --> G[Listening Configuration Wizard]
    G --> H[Select HTTPS Protocol and Port 443]
    H --> I[Bind SSL Certificate and Server Group]
    I --> J[Complete and Access through HTTPS Domain + Path]
```"
 # One-Click Deployment Guide

## # Goals and Preparations

In this tutorial, we will guide you on how to configure and install your own server, using a pre-configured server to achieve one-click deployment of a case in the VisualLogic editor. Once successfully deployed, you can access your application via IP+port or a configured domain name. You can also remove the deployed application through server deployment application management.

Before starting the deployment, you need to have login access to the server. Since software installation and service configuration operations are required, if you are not a root user, you need to have sudo privileges without a password prompt.

In this tutorial, we will use a Tencent Cloud server (rsa key login) and an Alibaba Cloud server (root account password login), both running Linux. Windows servers are not covered in this tutorial.

## # Server Configuration Steps

### # 1. Add Server

Multiple servers with the same IP cannot be created. The server provider is only for recording information and will not affect subsequent deployment rules.

### # 2. Server Installation and Initialization

Once created, you can initialize from the popup or select the corresponding server from the list to perform "initial configuration".

#### # Install Client Program

Install the client program using two methods (root account password, rsa key login).

- **Port and Domain Configuration**: Both server installation methods require port and domain configuration. Whether you need https access depends on your requirements. If https access is needed, parse the domain beforehand, referring to "[Configure Domain](https://www.ivx.cn/docview?page=dns-https#)".

- **Root Account Password**: For account password login, provide the corresponding account password information.

- **RSA Key**: For key-based server login, enter the text content of the key (e.g., .pem file) into "verification file".

- Once the information is completed, begin the installation. After successful client installation, proceed to the "Configure Server Resources" screen.

#### # Configure Server Resources

Includes relational databases, redis, file storage, and access control.

- **Relational Databases**: Two installation methods provided. If no database exists, a default one will be installed on the local server. If using a private database, fill in the required information.

- **Redis**: If no redis exists, a default one will be installed on the local server. If using redis, fill in the required information; the password can be empty.

- **File Storage**: Supports using or not using third-party storage. If using third-party storage, fill in the required information. For more on object storage details, refer to: [Object Storage Details](https://www.ivx.cn/docview?page=oss#%E7%9B%AE%E6%A0%87%E4%B8%8E%E5%87%86%E5%A4%87).

- **Access Control**: Has default initial values but can be modified.

Once the server configuration is completed, click next. Wait for the screen to prompt successful initialization configuration, then perform one-click deployment in the editor.

### # 3. Case Deployment

Open the case you want to deploy in the editor and click File -> Private Deployment -> One-Click Deployment, and follow the prompts. Once deployed, you can successfully preview the case by clicking the post-deployment application address.

For https access paths, access with a domain name; for non-https access deployments, use the IP.

### # Remove Deployed Application

In the server list, find the server used for case deployment, click "Deployment Application Management" at the bottom, enter the application list page, and remove the application.

Once all deployed applications are removed, the private server can be deleted.

### # Common Deployment Failure Causes

1. **Installation Failed Timeout**: Most likely due to insufficient bandwidth, causing a timeout when downloading mysql during initialization. You can temporarily increase the server's network bandwidth to above 50Mbps.
   
2. **mysql:not config**: Indicates that the database was not configured; check the database configuration.

3. **Port 80 Occupied**: If port 80 is occupied, you can change the HTTP port to another port, but ensure the corresponding modified port is open in the security group to allow successful HTTP access.
   
4. **ERROR! MySQL server PID file could not be found!**: On some servers, the database may need manual installation before proceeding with the one-click deployment process.


```mermaid
graph TB
    A(Goals and Preparations) --> B(Add Server)
    B --> C(Server Installation and Initialization)
    C --> D(Install Client Program)
    D --> E(Port and Domain Configuration)
    D --> F(Root Account Password Login)
    D --> G(RSA Key Login)
    C --> H(Configure Server Resources)
    H --> I(Relational Databases)
    H --> J(Redis)
    H --> K(File Storage)
    H --> L(Access Control)
    C --> M(Next Step)
    M --> N(Successful Initialization)
    N --> O(Case Deployment)
    O --> P(Successful Deployment)
    P --> Q(Access Methods)
    O --> R(Removal of Deployed Applications)
    B --> S(Common Deployment Failure Causes)
    S --> T(Installation Failed Timeout)
    S --> U(mysql:not config)
    S --> V(Port 80 Occupied)
    S --> W(ERROR MySQL server PID file could not be found)
```

### Standalone Deployment 1: Start VisualLogic Application

#### Install mysql and redis

mysql and redis are the underlying databases that VisualLogic applications rely on. In large-scale commercial applications, it is recommended to use cloud-hosted mysql and redis services, such as Alibaba Cloud's RDS and cloud database redis edition. Detailed access methods will be introduced in subsequent chapters. For ease and cost-effectiveness, this tutorial will use mysql and redis installed on a local server. You can use mariaDB as a substitute for mysql, as they are essentially equivalent.

Minimum versions for the two software are:

- mysql: 5.7.9
- redis: 2.8

In centos, use the yum command to install both software. Refer to: [CentOS7 install mysql](https://www.yuque.com/docs/share/c7c348a9-6d88-4924-8f60-bc2ae49b8eb3) and [CENTOS7 install REDIS](https://www.yuque.com/docs/share/d850a2ef-689d-4203-bede-749ba4275316).

#### Create Directory and Upload Program Files

1. **SSH Connection to Server**

   It is recommended to use SSH to log into the Linux instance. In the terminal, enter and execute the following command to connect to the Linux server:

   ```bash
   ssh <username>@<hostname or IP address>
   ```

   Enter the obtained password and press Enter to complete login. After a successful login, you can use the following command to view files in the current path:

   ```bash
   ls
   ```

2. **Create a New Folder**

   Create a new directory in the current server path, for example, "testA":

   ```bash
   mkdir testA
   ```

3. **Download Base and Application Files**

   Ensure that there are no Chinese characters in the path where the files are downloaded. Use the scp command to upload files to the server:

   ```bash
   scp <filepath> <username>@<IP address>:<path/>
   ```

4. **Set ivxbase as Executable**

   Switch to the testA directory and set ivxbase as executable:

   ```bash
   cd testA/
   chmod +x ivxbase
   ```

#### Initialize ivxbase

1. **Run ivxbase**

   Enter the following command in the current directory to run ivxbase:

   ```bash
   ./ivxbase
   ```

2. **Fill in Configuration Items**

   Specify the port, using a custom port and avoiding port 80. For example, use port 9966. Provide the addresses and user information for redis and mysql, using the locally installed redis and mysql.

3. **Access Application**

   After running, the command line will print the application path. You can access the application using IP+port:

   ```bash
   http://<IP>:<port>/play/<path>
   ```

   You can stop the current process by clicking Ctrl+C in the terminal.

```mermaid
graph TB
    A(Install mysql and redis) --> B(Create Directory and Upload Program Files)
    B --> C(SSH Connection to Server)
    B --> D(Create New Folder)
    B --> E(Download Base and Application Files)
    B --> F(Set ivxbase as Executable)
    F --> G(Initialize ivxbase)
    G --> H(Run ivxbase)
    G --> I(Fill in Configuration Items)
    I --> J(Access Application)
```

### One-Click Push and One-Click Deployment

In the privately deployed VisualLogic system, you can integrate with the company's internal k8s container cluster to push VisualLogic applications to testing and production environments with one-click.

#### 1. Configure Image Repository

Enter the image repository from the workspace, then configure the deployment service address on the repository settings interface. Each company may have a different environment deployment structure, so configuration is needed in the image repository. Additionally, you can add repository administrators who can review deployment requests for the production environment.

#### 2. Push Application Image Package

After completing an application development in the VisualLogic editor, you can choose to push the deployment package. Specify the application name and version description, and the deployment package will be pushed to the image repository. VisualLogic will complete the following tasks:

- Export and compile java source code
- Compile and upload docker image
- Generate helmchart deployment package

If the current application is a sub-application within a group application, the deployment package will include all applications in the group.

##### Common Issues

1. **Java Source Code Compilation Failure**: Since the online preview environment uses a real-time code parsing mode without compilation, there may occasionally be compilation failures. If compilation fails, you can use the prompt to locate the problem event block. If unresolved, export the source code and compile manually.

#### 3. Deploy Applications in Image Repository

After a successful push, refresh the image repository to see the just-pushed deployment package. Select one-click deployment, and a deployment configuration popup will appear.

##### Deployment Configuration Items

1. **Deployment Environment**: Choose between the test or production environment. The addresses of the test or production environments are designated in system configuration, so users do not need to specify them during deployment. If deploying to a production environment, administrator approval is required.

2. **Environment Variables**: If the application specifies environment variables, all variables will appear in the deployment configuration, allowing modifications during deployment.

3. **Container Specifications and Scaling**: Choose container configurations; it is recommended to use at least 1 core and 2GB configuration for the test environment, with higher configurations selected based on need for the production environment. Configure scaling rules as well, so when CPU/memory exceeds certain levels, automatic scaling occurs.

4. **Backend Resource Configuration**: Includes mysql, redis, file system, and other backend resource addresses. The default configuration is stored during VisualLogic deployment and usually does not require modification.

Once configured, choose one-click deployment to start the k8s container, where you can view container statuses and logs.

#### 4. Application Update and Rollback

The application update and rollback process are the same as the new deployment process; in the one-click deployment interface, select a different version to deploy. Old containers will not be destroyed before the new container starts, and the application remains accessible.

#### 5. Logs and Monitoring

VisualLogic's backend logging capabilities can integrate with various cloud or local logging services. Besides viewing real-time logs in the container, you can also view and analyze historical logs in the logging service.

#### 6. Cancel Deployment

For deployed applications, you can choose to cancel deployment. After cancellation, the k8s services, routes, and other application-related resources will be destroyed together. For applications that have been canceled, you can choose to further remove the deployment package.

```mermaid
graph TB
    AA(Configure Image Repository) --> AB(Push Application Image Package)
    AB --> AC(Deploy Applications in Image Repository)
    AC --> AD(Application Update and Rollback)
    AD --> AE(Logs and Monitoring)
    AE --> AF(Cancel Deployment)
```

In conclusion, by following these steps, users can easily achieve one-click deployment and push using the VisualLogic tool, improving development efficiency and enabling rapid iteration and deployment.
 # Recommended Server Configuration

### Private Deployment of Applications

Refers to exporting VisualLogic-developed applications for private deployment on your own managed servers. You may refer to the following recommended configurations:

| Operating System                                             | Processor        | Server                                           | Number of Servers          | Suggested Server Configuration | Software Configuration (Optional Installation) | Supported Cloud Platforms                              |
| ------------------------------------------------------------ | ---------------- | ------------------------------------------------ | -------------------------- | ------------------------------ | ---------------------------------------------- | ------------------------------------------------------ |
| Linux/Windows Recommended OpenEuler 22.03 supports Local Kylin | x86 Supports ARM | Supports Docker+K8S Private Server, Cloud Server | 1 server or 1 Docker image | CPU >= 2 cores, Memory >= 4 G  | MySQL 5.7 or above, Redis 4.0 or above         | Alibaba Cloud, Huawei Cloud, Tencent Cloud, AWS, Azure |

The following are optional middleware, configure as needed:

| Type         | Support                                                      | Production Environment     |
| ------------ | ------------------------------------------------------------ | -------------------------- |
| File Storage | Alibaba Cloud OSS, Tencent Cloud OSS, Amazon S3, Microsoft Azure, Minio, Huawei Cloud OBS | Recommended to configure 1 |
| CDN          | Alibaba Cloud CDN, Tencent Cloud CDN, etc.                   | Recommended to configure 1 |

### Private Deployment of Tools

Refers to deploying the entire VisualLogic development platform (including the editor, workbench, etc.) into the enterprise's internal development environment. The required configurations are as follows:

| Configuration Item                             | Configuration Value                                          |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Operating System                               | Linux/Windows Recommended OpenEuler 22.03 version, supports Local Kylin |
| Processor                                      | x86 Supports ARM                                             |
| Server                                         | Private Server, Cloud Server (ECS, etc.)                     |
| Number of Servers                              | 1 server                                                     |
| Suggested Server Configuration                 | CPU >= 8 cores, Memory >= 16 G                               |
| Software Configuration (Optional Installation) | MySQL 5.7 or above, Redis 4.0 or above *MySQL is recommended to be installed on another server *oss is suggested for file storage |
| Supported Cloud Platforms                      | Alibaba Cloud, Huawei Cloud, Tencent Cloud, AWS, Azure       |

### Additional Notes

1. For private deployment of applications, VisualLogic is free of charge, and developers can complete it by referring to the documentation.
2. Since CentOS has stopped maintenance, it is no longer recommended. Domestic users can choose OpenEuler 22.03, which is more in line with local and information technology innovation requirements.
3. Customization and deployment of the VisualLogic development platform is only provided for enterprise users. For detailed quotations, please consult via WeChat.
4. It is recommended to use the Chrome browser to access VisualLogic-developed tools and web applications.
 # Summary of Common VisualLogic Issues

#### The Position and Definition of VisualLogic in the Technological Field:

VisualLogic is a new form of "program logic" expression, which we can refer to as "VisualLogic." It is also a "new way of programming," enabling development of various front-end and back-end applications through "graphical/visual" means (drag and drop), and can automatically generate front-end and back-end code. Similar to how ChatGPT generates code through textual descriptions, VisualLogic describes through graphical means. Strictly speaking, VisualLogic can be classified as a "graphical programming language," featuring a complete AST and compiler/parser, facilitating automatic conversion with other existing programming languages.

#### Advantages of VisualLogic Programming Over Traditional Code Programming:

Compared to traditional high-level programming languages (such as C/C++, Java, JavaScript, C#, etc.), VisualLogic eliminates programming syntax during development, retaining only the program logic, and automatically generates code in various languages through graphical methods. This characteristic allows developers to "learn faster," "develop faster," and "maintain faster." Additionally, VisualLogic employs "cloud computing" technology, decoupling application development from the backend runtime resources. VisualLogic emphasizes the "shortest path" to application development, achieved through implementation and comprehensive "visual logic" divided into components.

#### Quality of Code Generated by VisualLogic:

The core components of VisualLogic, including "front-end compiler" and "back-end compiler," are developed by experts in the Linux startup field. While we cannot guarantee the generated code is optimal, user feedback indicates that the efficiency of VisualLogic-generated code surpasses that of most handwritten code. Technologically, VisualLogic incorporates advanced concepts like "cloud computing," "front-end and back-end separation," "front-end data driven," "containers," "microservices," and "serverless."

#### Can VisualLogic Support Development of Medium to Large-Scale Applications?

VisualLogic can fully support the development of medium to large-scale applications. It has been used to develop and iterate its own website and related systems, including core products and management systems for publicly traded companies. Examples include airport core management systems, industrial IoT MES systems, financial systems, and government systems. With over 1 million systems online and operational, VisualLogic demonstrates its applicability and reliability in large-scale system development.

#### Why is VisualLogic's IDE Free for Ordinary Developers?

VisualLogic aims to become a programming language, and we believe this language should be free and perpetually so for individual developers. Costs are only incurred when users opt to use VisualLogic-provided cloud computing resources. Whether or not users use VisualLogic's cloud computing is entirely their decision; opting out does not affect any functionality, and users can export or "one-click deploy" their developed applications to any other cloud computing platform.

#### Who is VisualLogic Suitable For?

In general, VisualLogic is suitable for all individual developers and companies. Currently, the primary users include students in computer-related fields, professionals transitioning to programming, product managers, and data analysts. Corporate users include medium to large state-owned enterprises, government and enterprise sectors, startups, and IT outsourcing companies looking to increase development efficiency, reduce costs, enable digital transformation, and achieve rapid prototyping and iteration.

#### Can VisualLogic Apply for Copyright?

Yes, VisualLogic is equivalent to a programming language, and the generated code fully belongs to the developer. VisualLogic provides regulatory functions and ensures positive technological evolution in accordance with national requirements.

#### Can Applications Developed with VisualLogic be Exported for Deployment? Can the Entire Platform be Deployed Independently?

Applications developed with VisualLogic can be exported as source code and feature a "one-click deploy" function for maximum user convenience. The VisualLogic platform also offers external deployment services for the entire platform, and users can contact the official team for more information.

#### Where in the VisualLogic Development Process Can Code be Written or Used?

In the VisualLogic system, all code is "sufficient but not necessary," meaning code resources can be used wherever needed. Users can:

- Customize components, write their own code, or download npm packages;
- Add custom CSS;
- Introduce various SDKs, such as Java, JS, Android, Python, etc.;
- Define front-end JS functions;
- Generate SQL code or manually add SQL code to run.
 # VisualLogic front-end source code generation and export

### Frontend Source Code Export Introduction

The frontend source code exported by VisualLogic is based on VisualLogic's proprietary frontend data-driven framework. This framework separates the data computation logic from the underlying virtual DOM rendering logic, providing better rendering performance and flexible usage. It extends support for features such as local variables within loop containers and advanced loop containers (like two-dimensional table containers and tree containers). The basic format of the framework is similar to Vue, so developers with experience in Vue or React can easily adapt and further develop the exported source code.

Note that to better utilize existing code formatting plugins, the main file of the exported project is named `app.visuallogic.vue`, but the project itself is not a Vue project; it is based on the VisualLogic engine and cannot be compiled using the Vue engine. However, any VisualLogic project, page, or small module can be packaged as a Vue or React component and used in Vue or React projects.

### VisualLogic Code Generation Process and How to Further Develop

#### Code Generation Process

The main process of VisualLogic code generation is divided into three steps:

1. Abstract and construct the "atomic components" of the frontend and backend, which have basic "trigger conditions", "properties", and "functions".
2. Use the "Logic Panel" to combine all "atomic components" and "functional logic" to form an "Abstract Syntax Tree (AST)". Complete logic construction can be done without any code through operating the Logic Panel.
3. Finally, generate the code, with the supported formats mainly including JavaScript and SQL.

The "Logic Panel" of VisualLogic is "Turing complete", theoretically able to express all reasonable logic through the Logic Panel, allowing for no-code development.

```mermaid
flowchart LR
  A[Abstract and construct frontend and backend atomic components] --> B[Logic Panel forming AST]
  B --> C[Generate code]
```

#### Running Environment for Generated Code

The frontend can generate JavaScript or native WeChat Mini Program code, which can run in their corresponding system environments. JavaScript code generated for the middleware can run on servers, such as in the Deno environment, with multi-threaded execution supported. Additionally, the generated SQL statements can be executed in databases like ElasticSearch and MySQL. Using technologies like Docker, this code can be flexibly deployed and efficiently run.

#### Which Code Can Be Modified

The JavaScript code generated by VisualLogic for both the frontend and middleware can be directly modified. However, it is recommended not to manually modify the auto-generated JavaScript code, as it will not be re-importable into the VisualLogic system.

#### Further Development

Further development is best conducted directly within VisualLogic, avoiding bypassing the initial development process. Iterative development through VisualLogic is more convenient and efficient than manually modifying the source code. VisualLogic allows developers to embed their own code to implement specific functions within the application.

### Source Code Export Entry

In VisualLogic 5.0, applications support exporting source code for Java projects based on Spring Boot:

1. A complete Java project, including full backend code and frontend files, which can be directly compiled and run. If the current application belongs to a group application, the complete Java project will export all the code and frontend files of the sub-applications within the group.
2. Only application code files, which include only the backend code related to the current application. After export, these can be unzipped into a previously exported complete project to update the backend code of the current application. In a group application, exporting application code files will only export the code of the current sub-application.

### Frontend Technology Framework

The core frontend framework of VisualLogic is based on a data-driven architecture and includes partially self-developed object-driven engines. The framework also incorporates multiple JavaScript libraries such as pixi.js, three.js, phaser.js, and babylon.js.

### Preview and Publish Modes

VisualLogic's web applications are divided into preview mode and publish mode. Both modes behave the same during playback, but the preview mode uses a WebAssembly engine for real-time parsing to sync with the editor content. In contrast, the publish mode employs full compilation for performance and code security.

```mermaid
flowchart LR
  A[Editor Content] --> B[Preview Mode]
  A --> C[Publish Mode]
  B --> E(WebAssembly Engine for Real-time Parsing)
  C --> F(Fully Compiled JS Code)
```

#### Data-Driven VS Object-Driven

In a data-driven environment, the display attributes of UI controls are bound to a data source, and changes in the data source reflect in the UI controls. By configuring data binding relationships, changes to the data source alter the display behavior of UI controls. The interaction loop in a data-driven environment is as follows:

1. UI controls bind to a data source and initialize their display based on the data source.
2. UI controls listen for interaction events.
3. Appropriate actions (functions) are called, processing a series of logic and changing the data source.
4. Modify the display behavior of UI controls.

Conversely, in an object-driven environment, UI controls do not directly bind to a data source. Instead, display behavior changes by directly setting display attributes, with the interaction loop as follows:

1. UI controls listen for interaction events.
2. Appropriate actions (functions) are called, processing logic, directly manipulating the UI control objects, and changing display attributes.

An object-driven environment is suitable for applications with complex front-end interactions like animations and games, while a data-driven environment is more apt for applications with frequent back-end data interactions, such as OA systems, e-commerce, CRM, IoT, and financial management systems.

### Architecture and Technology of VisualLogic-Generated Frontend Code

The frontend code generated by VisualLogic adopts a proprietary framework, including a data-driven layer and a virtual DOM rendering part. This framework supports React and Vue and uses Webpack bundling, micro-frontend technology, and various JavaScript libraries like Three.js and Phaser.js.

### Architecture and Technology of VisualLogic-Generated Backend Code

The core framework of VisualLogic's exported code is Java SpringBoot. The deployment of the project requires a dependent environment, including JDK, Redis, databases (like MySQL), and Nacos. It supports Docker packaging and deployment, realizing efficient operation in a flexible cloud environment.

```mermaid
flowchart LR
  A[Export Code] --> B[Frontend]
  A --> C[Backend]
  B --> D[Supports React and Vue]
  C --> E[SpringBoot Framework]
  E --> F[Depends on JDK and Redis]
```

### Quality and Efficiency of Generated Code

The code generated by VisualLogic is concise and efficient. It can generate frontend code for the Vue/React framework and backend code for the Java SpringBoot framework. During development, WebAssembly technology is used for preview, while the release versions adopt a compiled method, ensuring fast preview during development and efficient operation during release.
 # Introduction to VisualLogic Programming Box

# VisualLogic Programming Box Introduction

## Overview

### Why is VisualLogic making hardware?

The VisualLogic Programming Box integrates its core capabilities into a hardware product, providing the best cost-effective service to universities, training institutions, and educators. The hardware serves as a carrier for the core VisualLogic system, but the service content and quality remain unchanged. This approach combines the characteristics of VisualLogic, supporting WebApp development and various website/web application development, including mobile, but does not support WeChat mini-programs, native applications (iOS/Android), or desktop applications (Windows) development.

### What are the characteristics of VisualLogic?

The VisualLogic visual programming platform serves developers and is akin to a new programming language or graphical development paradigm. Its main feature is the generation of full-stack code with great flexibility, differing from other low-code platforms. VisualLogic can achieve unification and compatibility with existing development systems at the code level. The idea of developing the VisualLogic Programming Box stems from this.

### Why make it into the current hardware box (mini PC)?

1. **Data Security**: Supports intranet development, local data storage, and provides secure backup solutions.
2. **Convenient Development**: Portable, enabling development, debugging, and deployment anytime, anywhere.
3. **Cost-effective and Efficient**: Supports 50 developer accounts, reduces reliance on cloud computing resources, and saves costs.

## Usage Scenarios

Cases where the use of the VisualLogic Programming Box is not recommended include:

- Development teams with more than 50 developers.
- Projects requiring the development of mini-programs or local native applications for iOS/Android/Windows.

## Expandable Features

- **Developer Performance System**: Tracks the complexity and duration of each developer's work, quantifying their work status.
- **Demo System**: Provides hundreds of demos for learning and internal sharing.
- **Doc Documentation System**: For documentation and reference.
- **Free Video Tutorials**: Available for learning and reference.

## Conclusion

When using the VisualLogic Programming Box to develop large projects or complex application scenarios, it is recommended to contact the platform for support and solutions. Exploring more demos and learning resources will help you quickly get started and improve your efficiency.

For any questions, feel free to consult or learn more on the official website.
 # Theme Styles

### Overview

With the continuous evolution of software development, users have an increasing demand for personalized and aesthetic interface styles. Against this backdrop, the VisualLogic platform has launched the "Style Theme" feature, which not only enhances the richness and freedom of basic component styles but also significantly accelerates development efficiency. Below is a detailed introduction and usage guide.

### Feature Entry and Interface

#### Feature Entry

In the IDE development interface of VisualLogic, users can easily find the "Theme Configuration" button in the function bar. Clicking this button brings up the theme configuration pop-up. When creating a new case, users can also select the desired theme in the new window.

#### Interface Introduction

The theme configuration window consists of the following five areas:

1. **Current Theme Area**: Displays the theme applied to the current case, allowing users to select and modify its configuration. These configurations include the theme's name, theme color, and the components that need to be applied. Users can also add, edit, delete, and set defaults for each component's style. If a theme applies to multiple cases, users can convert it to a "Cloud Custom Theme" through the "Upload Theme" button.
2. **Search Theme Area**: In this area, users can search for official and cloud custom themes simply by entering the corresponding name.
3. **Official Preset Theme Area**: This area provides some officially preset themes, customized based on development habits within the platform, and updated periodically.
4. **Cloud Custom Theme Area**: This area lists cloud custom themes within the personal or enterprise scope, which are different from official themes. Users can create new themes from scratch.
5. **Theme Editing Area**: Used for editing cloud custom themes, this area is similar to the current theme area but includes an "Delete Theme" button. Since these themes are private configurations of the user, they are allowed to be deleted.

### Usage Process

1. **Create a Custom Theme**: Enter the theme name and theme color and confirm to create a brand new blank cloud custom theme.
2. **Add Component Style**: Initially, the theme has no content, so you need to select necessary components through the add button on the right, which will be displayed in the list.
3. **Define Component Style**: By clicking the button to the right of the component type, you can create a new style for that component type. Each style must have a unique name for direct replacement during style switching.
4. **Real-time Editing and Preview**: The editing interface attempts to restore all attribute styles of the component, and these modifications can be viewed in real-time in the preview window. To maintain readability of the editing window, the maximum width and height of the preview window are limited to 560px*80px.
5. **Apply and Switch Styles**: After completing the style editing, clicking confirm will add the style to the theme. Clicking "Use Style" allows the cloud style to be applied to the case. When you need to switch styles, users can select the "Style" attribute in the component's attribute panel for switching.
6. **Custom Style**: If any attribute in the style is modified, that component's style becomes a "Custom Style".

```mermaid
flowchart TD
    A[Create Cloud Custom Theme] --> B[Add Component Style]
    B --> C[Define Component Style]
    C --> D[Real-time Editing and Preview]
    D --> E[Apply and Switch Styles]
    E --> F[Custom Style]
```

### Theme Switching Instructions

Theme switching refers to the overall replacement of the theme content for the current case. During this process, VisualLogic's IDE will detect the style settings of all components with styles in the case. When component names are the same and use the same style names, the system will automatically replace them with styles from the new theme, thus reducing the user's manual operations.

The above is a detailed explanation of the "Style Theme" feature on the VisualLogic platform. This feature is designed to provide users with greater design freedom and development efficiency, fully meeting the diverse needs of different users for interface styles. Through this, VisualLogic once again proves its advantages and innovations in the no-code development field.
 # How to do SEO

## VisualLogic Application SEO

VisualLogic applications are developed based on Single-page Applications (SPA), which means all page elements are dynamically rendered via JavaScript rather than static HTML. This architecture enhances user interaction experience, but poses challenges for search engine crawlers, as crawlers more easily parse static HTML.

## Single-page Application SEO Principle

Since the content within VisualLogic applications is dynamically generated, it is not crawler-friendly. To address this issue, VisualLogic offers a page pre-rendering service that detects if a request comes from a search engine crawler and renders the page on the backend as static HTML, ensuring crawlers get the full page content.

## Enabling Application SEO

**Step 1: Enable Application SEO Configuration**

In the application configuration interface, select the SEO configuration, turn on the SEO switch, and save the settings to initiate the pre-rendering mechanism. Note that the SEO feature is only effective for published versions of the application. Once enabled and the settings are saved, the published version of the application will automatically include the pre-rendering feature, obviating the need for republishing.

In the SEO configuration, you can set the cache duration (1-7 days). The default no-cache setting means the backend will re-render every time a crawler request is received. With caching enabled, the backend will return the previously pre-rendered page, improving response speed. The duration of the cache should be determined based on the application's update frequency.

**Step 2: Set Pre-rendering Timing**

If page content is dynamically fetched, pre-rendering should occur after data retrieval is completed to ensure completeness. Use the "Application System" component to set pre-rendering timing. If not set, the system will pre-render 3 seconds after page initialization, which may result in crawler timeout failures.

The pre-rendering action should be triggered by automatic events, not user operations, as crawlers do not execute user actions.

```mermaid
flowchart TD
    A[Enable SEO Switch] --> B[Configure Cache Duration]
    B --> C[Set Pre-rendering Timing]
    C --> D[Initiate Automatic Trigger Events]
```

## Optimizing SEO: Adding Hyperlinks

To enhance crawler's ability to index multiple pages within the application, add hyperlinks. Assign a different URL address for each subpage and use hyperlink components to create page navigation effects, allowing crawlers to access different pages via the `<a>` tag. Use relative addresses for linking to modify the URL without refreshing the application.

## View Pre-rendering Effect

After enabling SEO, you can simulate a crawler and view pre-rendering effects by changing the browser's user agent. In Chrome, open the console and select `Googlebot`, then refresh the browser to view the static pre-rendered pages. Note that this behavior is akin to a "screenshot" of the HTML, and interactive features will be disabled.

## Application Pre-rendering Costs

Once SEO is enabled, pre-rendering operation costs will be included in the detailed costs, covering both pre-rendering computation and traffic costs. If the application has caching enabled, only traffic costs are incurred during the cache period, with no additional pre-rendering costs.
 # WeChat Component - API Configuration

### Goals and Preparation

In the previous chapter, we implemented using an HTTPS domain format such as "https://xxx.com/xxx" on a PC browser. However, it couldn't be accessed properly in a WeChat browser, as WeChat requires the domain to be authorized by the service account. Therefore, the service account needs to be bound to the case, and the relevant settings modified.

### Service Account Settings

First, log in to the service account backend, click "Basic Configuration", and obtain the service account's appid and appsecret (appsecret can be reset if forgotten). Also, add the server's IP address to the "IP Whitelist".

Switch to "Feature Settings" in "Public Account Settings", and fill the root domain name for both business domain and JS safe domain (Note: business domain and JS safe domain can only be modified 5 times a month). Fill in the final subdomain that needs to be accessed for the web authorization domain. For example, if "https://wxauth.ih5school.cn/play/asIPshax" is needed, then fill wxauth.ih5school.cn for the web authorization domain.

The above are the settings for the WeChat public account (service account).

### VisualLogic Configuration Center Binding to Service Account

Enter the service account's appid and APP secret in the WeChat interface configuration in the Configuration Center, and fill in the custom authorization domain with the subdomain (consistent with the web authorization domain of the service account from the previous step).

After saving, re-export the case and deploy it privately to the server, so it can be successfully opened in the WeChat browser, such as "https://wxauth.ih5school.cn/play/asIPshax".

### WeChat Public Account

#### Overview

The WeChat public component encapsulates the WeChat web environment's jssdk, mainly used in the WeChat browser to call the related interfaces provided by WeChat. The functionalities include:

1. Calling WeChat's recording and sharing menu functions.
2. Fetching WeChat location information.
3. Calling WeChat payment interfaces.
4. Editing WeChat title, description, and other information.
5. Getting WeChat authorization to fetch user nickname, profile picture, and other information.
6. Setting custom public account and user authorization information.

Any cases involving the above application scenarios need to add the WeChat component. The WeChat public component can be added to H5 applications or mini programs.

#### Usage Scenarios

Most mobile-end interactive web pages and micro-sites need to be placed in the WeChat environment. Skillful use of the WeChat component can create rich interactive effects, such as obtaining a user’s nickname, profile picture, and other information through the WeChat component to realize data customization. Using WeChat's recording and voice recognition upload functions can create interesting interactive functions like voice postcards. Calling WeChat geolocation for precise navigation, and using the WeChat payment interface to accommodate more commercial scenarios.

#### Basic Usage

1. **Adding and Setting Basic Properties**:
   The WeChat public account component can be added to the foreground of PC applications or mini-program type cases, and only one component can be added per case.
2. **Function Examples**:

   - QR Code Scanning: When WeChat public account initiates "QR code scanning", it will call WeChat's built-in scanner. The scanning result value or failure reason can be obtained after scanning.

### Configuration for WeChat Mini Programs, WeChat Apps, etc.

Depending on different front-end usage scenarios, you need to apply for different WeChat application IDs and fill in the relevant information in the WeChat backend configuration.

#### Points of Attention

- WeChat Mini Programs (If payment-related functionality is needed, then merchant number information is required).
- WeChat H5 (If using payment features, the WeChat merchant number information is needed).
- WeChat APP for Android (If using payment features, the WeChat merchant number information is needed).

### Creating and Filling Configuration for WeChat APP

1. **Creating an APP Application**:
   Go to the WeChat Open Platform, create an APP application, and enable the related interfaces (e.g., WeChat payment).
2. **Management Center**:
   In the WeChat Open Platform "Management Center", create a "Mobile Application", fill in the necessary information, and proceed to the next step. Enter the APPID (application package name) from VisualLogic's Configuration Center Android tab, and the application signature generated after creating the KEYSTORE, then submit for review. After approval by WeChat, the newly created "Mobile Application" can get the APP ID.
3. **Filling Configuration Information**:
   In the WeChat APP component configuration, fill in the APP ID and APP Link, used for compiling and packaging the front-end APP.

   1. Add the "WeChat app interface" component and fill in the APP ID and APP Link (case address).
   2. The APPID filled in VisualLogic's Configuration Center Android tab must be consistent with the "Mobile Application" package name (otherwise the sharing function cannot be used). Copy and modify the "View Signature" to the application details in the WeChat Open Platform.
4. **Backend Configuration**:
   Besides filling in the configuration information on the front end, for using payment and login interfaces, additional configuration of APP secret is needed in the backend WeChat interface.

### Interface Configuration

Interface configuration is the centralized configuration content of VisualLogic for the front-end and back-end third-party interface components. Users can configure the relevant content of third-party interfaces here (such as mini-programs, maps, WeChat public account, etc.).

Users can select the relevant component name that needs to be configured, and configure the content for the interface. The configuration content area on the right allows users to fill in the specific interface configuration. After completing the configuration, click save. Users can name the configuration and check "Add to preset configuration when saving" before saving for reuse in different case applications, making it convenient for reuse with no or minor modifications.

### Backend WeChat

The backend WeChat provides various methods for calling WeChat backend interfaces, such as authorization login, payment, sending template messages, etc. Some functions need to be used in conjunction with front-end WeChat interfaces. The front-end WeChat interfaces are divided into three interface components based on different environments:

- WeChat Public Account: For H5 applications in the WeChat browser web environment, calling jssdk interfaces.
- WeChat Mini Program: For front-end mini-programs invoking mini-program front-end interfaces.
- WeChat APP: For ios/android APPs, calling the WeChat sdk encapsulated native interfaces.

In the sub-documents of this component, we will detail the usage methods of WeChat login, payment for each environment.
 # How to Use Environment Variables

### VisualLogic Environment Variables Overview

Environment variables are custom configuration information for applications, defined outside of the application code. Compared to ordinary in-application variables, a significant advantage of environment variables is their separation from application logic, allowing for flexible modification of application behavior without saving or publishing the application. This feature makes environment variables particularly suitable for configuration information that requires frequent adjustments. Additionally, when an application is published as a template or sent to other users, the values of its environment variables are not copied. Therefore, environment variables are also ideal for storing confidential information, such as application keys for third-party paid APIs.

### Use Cases for Environment Variables

1. **Separation from Application Logic**: Environment variables allow changes in application behavior without saving or publishing the application, which is particularly useful for frequently modified configuration information.
   
2. **Private Deployment Configuration**: In private deployments, environment variables can be set via the `vars` field in the configuration file `config.json`, allowing changes in application behavior without re-exporting the application code package. For example, when deploying a program in both test and production environments where the API request addresses are different, environment variables can be used to set different API addresses.
   
3. **Management of Confidential Information**: Since environment variables are not copied with the application or uploaded as a template to other users, they are particularly suitable for storing confidential information.
   
4. **Unified Management Interface**: A unified configuration management interface is provided to facilitate the maintenance and management of applications.

### Setting Environment Variables in the Cloud Platform

In the VisualLogic configuration panel, users can define and set environment variable values via the environment variables tab. Note the following points:

- **Available Scope**: Environment variables are divided into those available both on the front and back end, and those available only on the back end. Frontend variables are stored in the application's `index` file, so it is not advisable to store sensitive information here.
  
- **Version Distinction**: The values of environment variables can be distinguished between preview and release versions. The release version defaults to the preview version but can also be set independently.

- **Immediate Effect**: Once environment variables are saved, both the preview and release versions take effect simultaneously without additional save or publish operations. However, for frontend variables, the application needs to be refreshed for the changes to take effect.

### Setting Environment Variables in Private Deployments

In private deployment scenarios, environment variables are stored in the `vars` field of the `config.json` file. This setup allows setting common environment variables for all applications in the current deployment, a specific application group, or a single application. Here is an example of the vars field:

```json
"vars": {
  "*": {"a": "123"},   // Define global shared environment variables
  "g165723": {"a": "456", "b": "xxx"}, // Define group shared environment variables
  "n1762345": {"a": "ppp"} // Define in-application environment variables
}
```

In the above example, if an environment variable is accessed within the application with nid 1762345, the value of variable `a` is `"ppp"`; if it is another application belonging to the group with id 165723, the value of variable `a` is `"456"`; otherwise, the value of variable `a` is `"123"`.

**Note**: After modifying the `config.json` file, the VisualLogic interface program needs to be restarted for the changes to take effect.

### Referencing Environment Variables in Applications

Users can reference all currently set environment variables in the frontend or backend through the "Environment Variables" option in the formula editor dropdown menu. This makes the use of environment variables very simple and unified, effectively enhancing the efficiency of application configuration management.

```mermaid
graph TD;
  A(Configure Environment Variables) --> B[Private Deployment Setup]
  A --> C[Cloud Platform Setup]
  B --> D{config.json}
  D --> E[vars Field Configuration]
  C --> F[Front and Back End Available]
  C --> G[Back End Only]
```

Overall, the environment variables feature in VisualLogic provides great flexibility and security for application configuration management, helping developers manage and adjust application behavior more conveniently.
 # Backend Source Code Export (Java)

### Prerequisites

- Java needs to be installed on the computer (Java 8 or above is recommended) along with the appropriate environment configuration.
- A code editor is required (e.g., vscode).

**Preview Address**: [Click to Preview](https://v4pre.h5sys.cn/play/nRwf9z5J)

**Complete JAVA Project Demo Code**: [Download Link](https://file3.ih5.cn/v35/files/64576699e86675fa10620c79608ff2d0_1435741.zip)

### Introduction to Exporting Backend Source Code

In the new event panel mode of the VisualLogic editor, users can select "Export Backend Source Code (Java)" from the menu bar.

There are two options when exporting the source code:

1. **Complete Project**: Contains all framework-related code and the current application code, suitable for initial export.
2. **Current Application Code**: Only contains the code related to the current application, suitable for subsequent updates or adding new applications to an existing project.

**Java Source Code Main File Format**:

```
|- Project Root Directory
   |- gradle
   |- src
   |- build.gradle
   |- settings.gradle
   |- ...
```

### Exporting Backend Java Source Code

The backend supports Java source code export based on the Spring Boot architecture. Export and compile within the complete project code without relying on additional base programs. Microservices use the Dubbo RPC framework, supporting global transactions across services.

### Source Code Export Entry

In VisualLogic 5.0 applications, the export of Java project source code based on the Spring Boot framework is supported:

1. **Complete Java Project**: Includes all backend code and frontend files, ready to compile and run.
2. **Application Code Files Only**: Includes only the backend code related to the current application, allows quick export and update.

### Source Code Structure Explanation

**Exported Project Directory Structure Explanation**:

```
|- Root Directory
   |- src
       |- main
           |- java
               |- com
                   |- example
                       |- Service Files
                       |- ...
           |- resources
               |- Configuration Files
               |- ...
       |- test
       |- ...
```

Highlighted in red: Backend source code files for each application. Other files are system files and basic libraries.
Specific structure of the application source code files:

```
|- com
   |- Example Package
       |- Controller Files
       |- Service Files
       |- Microservices Files
```

### Backend Source Code Compilation and Running Process

After exporting the backend source code, it can be packaged into a zip file using Gradle, then upload the zip file to the server to run, along with the VisualLogic basic interface library, to achieve complete backend functionality. The specific process is as follows:

1. Export backend source code.
2. Compile and package using Gradle.
3. Upload the packaged file to the server.
4. Configure and run the backend service.

```mermaid
graph LR
A[Export Backend Source Code] --> B[Compile Using Gradle]
B --> C[Generate zip File]
C --> D[Upload to Server]
D --> E[Configure and Run]

```

### Detailed Steps for Source Code Compilation

#### Environmental Requirements

- Install JDK (version 8 minimum) and Gradle.

#### Case 1: First Download of the Project

1. After exporting the complete Java project, unzip the zip file.
2. In the command line, navigate to the project root directory and execute `gradle distZip`.
3. Upload the generated zip file to the server.

#### Case 2: Updating Application Code

1. Export "Application Code Files Only".
2. Replace the files in the original project with the newly downloaded files of the same name.
3. Execute `gradle distZip`.

#### Case 3: Adding/Removing Applications

1. Export "Application Code Files Only".
2. Add or remove code files.
3. Update the service entry files and execute `gradle distZip`.

### Detailed Steps for Program Deployment

1. **Upload and Run the Packaged File**

- Unzip and run `IvxJavaService`.

2. **Download and Upload the Application Export Package**

   The application export package contains frontend code and some basic configuration information of the service.
3. **Run the VisualLogic Basic Interface Library**

   Configure the port for the Java service and run the basic interface library.

```mermaid
graph LR
A[Upload and Run Packaged File] --> B[Download and Upload Application Export Package]
B --> C[Configure Port and Run Interface Library]
```

### VisualLogic 5.0 Updated Features

The backend supports exporting Java source code based on the Spring Boot architecture, allowing for the complete project code export and compilation. Future updates will support global transactions across services.
 # What is VisualLogic

#### VisualLogic Platform Overview

VisualLogic is a universal visual logic editing solution designed to organize various logic graphically and ultimately construct various "models" through "logic linking the world" to solve problems. VisualLogic processes traditional programming languages through "abstraction" and "structuring," reducing the learning curve, improving development efficiency, and simplifying operation and maintenance.

##### Core Technology and Working Principle

The core technology of VisualLogic is "Component-Oriented Programming (COP)." Its working principle is divided into three steps:
1. Abstract all componentizable entities into components, each containing trigger conditions, properties, and functions/methods.
2. Use the event panel and data flow panel to graphically express logic without writing code.
3. Combine components and logical expressions to generate front-end (Vue/React) and back-end (Java) code.

##### Advantages
- **Fast Learning**: No need to learn complex syntax and frameworks, just master component usage and logic orchestration.
- **Fast Development**: Operations generate a large amount of code, reducing development cycles.
- **Fast Execution**: The generated code is concise and efficient, with high runtime performance.
- **Fast Operations and Maintenance**: Supports CI/CD and DevOps, providing a complete enterprise-level operation and maintenance environment.
- **High Security**: Component mechanism isolates code, encrypts communication, integrates cloud platforms, and supports code encryption and scanning.
- **Strong Maintainability**: The generated code is highly readable, easy to maintain, and suitable for secondary development.

VisualLogic is essentially a complete graphical logic editing solution that quickly expresses application logic through rich components, event panels, and data flow panels, generating executable front-end and back-end code. Its goal is to solve problems through "logic visualization," expressing and transforming everyone's creativity into code, becoming applications or programs.

#### What is VisualLogic?

VisualLogic should be currently the simplest programming language, with its main feature being "graphical, automatic code generation (without writing code)." Its technology is mature and secure, committed to providing users with a new and complete visual development system. Based on logic visualization and full-stack code automatic generation technology, VisualLogic provides an easy-to-use and comprehensive platform for individual and enterprise developers, allowing front-end and back-end application and database development with just clicking, dragging, and configuring. It offers no-code and low-code visualization development capabilities, greatly reducing technical barriers and enhancing development efficiency.

##### Platform Introduction

VisualLogic represents a brand-new productivity tool that blends technological and product innovation, enabling application development with just clicking, dragging, and configuring. It offers various B2B solutions, including enterprise mid-platforms, digital management systems, smart IoT, and smart cities. VisualLogic ensures high-quality automatic front-end and back-end code generation, allowing application source code export and supporting private cloud deployment, avoiding lock-in.

##### Technical Domains

VisualLogic spans broad technical domains, including visual programming languages, IDEs, visual logic orchestration and page building, low-code/no-code, and more. The VisualLogic IDE is built upon an advanced programming language system, supporting front-end JavaScript code generation, and automatically generating languages like Java, Node.js, and Python on the back end. In addition, VisualLogic possesses custom extension and modular development capabilities, enabling insertion of program code, writing custom components, and introducing npm and SDK packages on a no-code development basis. It adopts a structured design, supporting cross-case and cross-account sharing and reuse of modules within the platform.

#### Innovation in Product Design

When VisualLogic began development in 2007, the team faced many challenges, needing to design a flexible architecture to cope with rapid technology iteration. For example, the syntax tree (AST) design for the graphical programming language was one of the difficulties, requiring assurance of generating popular programming languages and code frameworks.

After more than a decade of continuous optimization and refactoring, the team finally formed a satisfactory version and decided to bring it to the market. To date, approximately 300,000 registered developers have developed nearly 1 million applications through the VisualLogic platform. The code generated by VisualLogic performs excellently and is essentially bug-free, allowing users to use it with confidence.

#### Why We Developed VisualLogic?

VisualLogic aims to explore a new way of programming: replacing traditional code programming with graphical means, enhancing learning and development efficiency, and simplifying application operations and maintenance processes. It hopes to become an industry innovation benchmark.
 #  VisualLogic VS Low-code Platforms

#### Overview

VisualLogic distinguishes itself from other low-code platforms primarily through its technical approach and usage patterns. The core difference can be summarized as: VisualLogic focuses on "development state," generating full-stack code through drag-and-drop, while many other low-code platforms are oriented towards "runtime," generating applications through configuration.

##### Differences in Application Generation

- **VisualLogic**: The VisualLogic platform covers the entire application development lifecycle, ultimately generating complete code including front-end (Vue/React) and back-end (Java/Node.js), which developers can choose to export as source code. This technical approach ensures the reversibility of development, preventing users from being platform-bound.
- **Other low-code platforms**: Typically generate applications that can only run within their ecosystem, rather than independent source code. This limits application migration and flexibility, allowing users to customize and extend only under platform-permitted conditions.

##### Usage Scenarios and Technical Approach

- **Developer-friendly design**: VisualLogic offers Turing-complete programming capabilities, enabling complex logic through an event panel. By combining visual design and logic orchestration, it allows for complete application functionality without coding.
- **Enterprise-oriented service model**: Other platforms are often positioned as enterprise services, focusing on applications that facilitate rapid development and deployment within companies. These platforms impose technical architecture constraints from the outset, requiring developers to choose customized extensions in a restricted environment.

#### Technical Advantages and Innovations of VisualLogic

VisualLogic stands out from traditional low-code platforms by generating high-quality source code and providing a complete IDE, supporting integration with various programming languages. Compared to traditional coding, VisualLogic offers the following notable technical advantages:

###### Code Reversibility and Learning Curve

VisualLogic's technical reversibility grants developers high flexibility: developers can export source code and innovate at the code level when refining or adjusting functionality in the future. Compared to traditional high-level programming languages, VisualLogic learning is faster, with applications developed through visual component usage and logical connections.

###### Accelerated Development and Operations

- **Efficient Development**: Component encapsulation, a concise and intuitive event panel design, and the flexible coexistence of "code" and "non-code" shorten development time. VisualLogic's component design significantly enhances development efficiency, being 5 to 10 times faster than traditional coding.
- **Operations Advantage**: Based on cloud computing resources, VisualLogic achieves elastic resource management through Serverless architecture, supporting high concurrency scenarios without increasing operational costs.

###### Code Quality and Maintainability

The readability and high quality of VisualLogic-generated code facilitate convenient secondary development and maintenance. Even in rapidly changing technological environments, the code generated by VisualLogic remains sustainably utilizable due to its openness and strong extensibility.

##### Market Positioning and Target Audience of VisualLogic

VisualLogic is not only suitable for developers but also includes enterprise users. For enterprises, VisualLogic provides an integrated digital transformation solution, enhancing IT efficiency while ensuring corporate data security. For programmers, VisualLogic is an efficient code generator, and for non-programmers, it is undoubtedly an ideal tool for programming entry.

#### Conclusion

In summary, VisualLogic stands out in the low-code field by not only being easy to use but also generating highly readable complete code, meeting the diverse needs of both enterprise and independent developers. At the same time, the graphical IDE provided by VisualLogic reduces the learning curve, enabling more users to quickly engage in application development. Its powerful integration and extension capabilities combine the benefits of both traditional and modern programming, making it a promising next-generation development platform.
 # VisualLogic VS Blockly/Scratch

## Overview

**VisualLogic** is a universal graphical programming language equipped with a full-featured Integrated Development Environment (IDE). Its core concept is "component-oriented programming." Through "graphical logic orchestration," users can connect various components to form the complete logic of an application and generate independently executable application code via a compiler/interpreter.

**Scratch** and **Blockly** represent another "logic editing solution." Scratch is a children's programming platform centered around graphical "building block" logic. Blockly, invented by Google, is a new programming engine that supports a graphical programming style, capable of converting graphical building block structures into source code of various programming languages.

## Relationship between Scratch and Blockly

The relationship between Scratch and Blockly is that Scratch directly leverages Blockly's logic engine to create a programming platform suitable for children. It is noteworthy that Blockly itself is merely an engine and is widely applied across different fields, presented through various secondary development products.

## Blockly Interface and Features

Blockly is widely used for its graphical and visual features. Its core programming model is designed by dragging "building blocks" onto a language compilation conversion mode. Blockly provides a mechanism for extending multiple languages, which can be expanded by creating new blocks, defining block behaviors, or customizing the user interface.

## Advantages and Limitations of Blockly

1. **Low Development Efficiency**: The drag-and-configure model is less efficient compared to proficient coding, making Blockly less superior, especially when handling complex applications.
2. **Functional Limitations**: Closer to the "declarative programming" paradigm, with insufficient object-oriented encapsulation. Extending components is challenging, and it lacks the capability to handle complex operations like backend database management.
3. **Demand Gap**: Primarily suitable for children's programming and often unable to support the development of more complex, complete applications, leading learners to eventually shift towards code programming.

## Advantages Comparison between VisualLogic and Blockly

VisualLogic and Blockly/Scratch have their own strengths in "program logic editing solutions." VisualLogic, with its built-in IDE, allows developers to freely develop applications and manage the entire lifecycle on the platform. In logic editing, VisualLogic has its self-invented event panel. Compared to the building block drag-and-drop mode, this panel mode has the following advantages:

1. **Faster Development Efficiency**: Simpler operations that can generate a large amount of code in a single action.
2. **No Functional Limitations**: Equipped with a Turing-complete logic panel and layered component design, capable of handling complex applications and medium to large project development.

If this capability can be decoupled from the VisualLogic platform, developers could use the "logic panel" to construct their applications, regardless of the development environment, which is significant for the development of VisualLogic.

## Considerations for Decoupling VisualLogic Logic Panel from the Platform

VisualLogic is considering decoupling its logic panel from the platform, allowing other developers to use this new programming approach independently of the VisualLogic platform itself. This strategy is crucial for expanding the influence of VisualLogic and can help more developers utilize this "logic-to-code" capability to develop their applications.

From the above content, it can be seen that VisualLogic offers a more flexible and diverse development solution suitable for a wide range of developers, especially standing out in the development of complex projects compared to Blockly.
 # How to integrate SSO/Auth/Auth2

### Overview

VisualLogic, when integrating SSO (Single Sign-On), Auth (Authentication), and OAuth2 (Authorization), mainly relies on its powerful component-based programming paradigm. It uses a visual IDE along with various components to achieve integration with third-party authentication systems. This document will provide a detailed explanation of how VisualLogic completes the integration with third-party OAuth2 systems, including main modes and specific operational steps.

#### Third-Party OAuth2 Integration Modes

There are two primary modes for VisualLogic applications to integrate with third-party OAuth2 systems:

**Mode 1: Frontend UI with API Calls**

- Only the frontend UI is developed in VisualLogic, and when integrating with third-party systems, all backend services are implemented via third-party APIs.
- In this mode, users only need to complete the login operation of the third-party OAuth2 system on the VisualLogic frontend to obtain the API access token in their browser.
- After obtaining the token, the frontend API component of VisualLogic can be used to access various services provided by the OAuth2 system.

**Mode 2: Full Stack Integration Using Account System Components**

- In addition to the frontend UI, VisualLogic also develops the necessary backend services and authenticates with the third-party OAuth2 system.
- In this scenario, the VisualLogic account system component needs to be introduced. After completing third-party authorization, use the VisualLogic account system to generate a login token to control access permissions between VisualLogic backend services and frontend UI.
- A single VisualLogic account system can enable unified login for multiple VisualLogic applications, achieving the effect of SSO.

#### Detailed Steps for Frontend OAuth2 Login Integration

Regardless of the mode used, integration with OAuth2 login authentication is the first step. The following explains how to integrate a typical OAuth2 interface, using Microsoft's Azure unified authentication interface as an example.

Typically, the integration of an OAuth2 interface includes the following steps:

1. **Obtain Authorization Code**: Guide the user to visit a designated authorization page to acquire an authorization code.
2. **Exchange for Access Token**: Use the obtained authorization code to communicate with the OAuth2 provider and obtain the access token.
3. **Retrieve User Information**: Use the access token to access specialized interfaces to obtain detailed user information.

Specific steps (using Azure as an example):

1. Guide the user to the login page and set the correct redirect URL to acquire the authorization code.
2. After user authorization, the system will redirect back to your application with the authorization code.
3. Use this authorization code to request an access token. Ensure that the necessary parameters (such as client_id, redirect_uri, etc.) are included in the request.
4. After successfully obtaining the token, use this token to access the user information interface to get user details.

#### Post-login User Control and Permission Management

After the above process, generate a login token through the relevant functionalities of the VisualLogic account system for user identity and permission management.

For a self-built account system, VisualLogic provides the direct capability to call the system-generated login token, facilitating continued use of OAuth2 login in the future.

For role and permission differentiation:

- **Fixed Role Permissions**: The role permissions assigned to a user at the time of initial registration remain unchanged. Initial values for role fields can be set in the account system.
- **Dynamic Role Permissions**: Implement more flexible user permission management by developing the capability to dynamically adjust user roles.

Below is a flowchart of the OAuth2 integration process to help better visualize the steps outlined above:

```mermaid
graph TD;
  A[User Accesses OAuth2 Login Page] --> B[User Agrees to Authorize]
  B --> C[Obtain Authorization Code]
  C --> D{Authorization Code Acquired?}
  D --> |Yes| E[Request Access Token]
  D --> |No| A
  E --> F{Access Token Acquired?}
  F --> |Yes| G[Access User Information Interface]
  F --> |No| E
  G --> H[Use VisualLogic Component, Generate Login Token]
  H --> I[Implement Backend Service Permission Control]
```

#### Summary

VisualLogic, when integrating with third-party OAuth2 authentication, offers comprehensive support from frontend UI to backend services. Through component-based interface design and the VisualLogic account system components, developers can easily achieve unified identity management and permission control for multiple applications, greatly enhancing development efficiency and security. By following the above steps, effective integration of VisualLogic applications with third-party SSO/Auth/OAuth2 can be accomplished, regardless of the mode or system used.
 # Accessing the Programming Box from the External Network

# Technology Documentation for External Network Access of the Programming Box

## Overview
This document introduces how to access the Programming Box through an external network. It is important to note that the Programming Box can only be accessed in either external or internal network mode. When external network access is enabled, internal network access will be unavailable.

## Basic Usage
### 1. Download and Install Peanut Shell
Visit the following link to download and install the Peanut Shell tool: [Peanut Shell Download Page](https://hsk.oray.com/download/)

After installation, open the software and select the "Add Mapping" function.

In the configuration for adding mapping:
- **Internal Host Address**: Enter the local IP address of the Programming Box, which should be in the format 192.168.xx.xxx.
- **Bandwidth Setting**: The default bandwidth is 1Mbps. For faster access speed, you can upgrade to 5Mbps for a fee.
- **https Option**: When selecting https access, real-name authentication and payment for the authentication fee will be required.

### 2. Modify the Programming Box’s configuration.json
- Use SSH software (such as the recommended Xshell and Xftp) to connect to the Programming Box.
- The connection address is the IP of the Programming Box, with the default login username as root, and the password as VisualLogicSA-130-xxxxxxxxxx (the xxxx part is the S/N code of the box).
- Execute the following commands to enter the target directory and edit the configuration file:
  - `cd /opt/visuallogicsa`
  - Use a text editor to edit the `configuration.json` file.
  - Replace all values related to the 192.168.xx.xx address with the external network address generated by Peanut Shell.
- After making the changes, restart the server, wait for about a minute, and access the Programming Box through the external network address provided by Peanut Shell.

## Notes
1. **Bandwidth and Access Speed**: If you are still using the default 1Mbps bandwidth without an upgraded purchase, network access will be very slow, with the workspace taking about 4 minutes to load. Therefore, it is recommended to upgrade the Peanut Shell bandwidth and purchase a data package.
2. **Peanut Shell Client Running Status**: Ensure that the Peanut Shell client is running. If the client is closed, the Programming Box cannot be accessed through the external network.
3. **Restoring Internal Network Access**: To restore internal network access, revert the external IP address in the `configuration.json` file to the default local IP, and restart the Programming Box.

## Further Reading
For more details on the operation of the Programming Box and Peanut Shell software, common challenges, and solutions, please refer to Peanut Shell's technical support documentation or community forums for more in-depth guidance and examples.
 # Cloud Platform Data Backup

### Overview

The applications created on the cloud platform using VisualLogic have their code stored in the AWS S3 file storage service. Each application stores a version when manually saved, with a maximum of 200 versions. Thus, applications under normal development naturally have multiple code backups. Users can also manually save application code to prevent accidental deletion or mishandling. If issues arise with a code version, users can load a backup version or a saved application at any time without additional recovery wait time.

Every time an application is published, the code and related runtime files are automatically uploaded to the release environment (production environment), which is independent of the preview environment (development environment). Each published version has an independent set of code files stored in the production environment. Additionally, AWS's file storage offers multi-region availability, with data security and backup management handled automatically, requiring no extra effort from the user.

### Code Backup

The applications created on the cloud platform using VisualLogic have their code stored in the AWS S3 file storage service. Each application stores a version when manually saved, with a maximum of 200 versions. Thus, applications under normal development naturally have multiple code backups. Users can also manually save application code to prevent accidental deletion or mishandling.

If issues arise with a code version, users can load a backup version or a saved application at any time without additional recovery wait time. Every time an application is published, the code and related runtime files are automatically uploaded to the release environment (production environment), which is independent of the preview environment (development environment). Each published version has an independent set of code files stored in the production environment. Additionally, AWS's file storage offers multi-region availability, with data security and backup management handled automatically, requiring no extra effort from the user.

### Code Encryption

Any code files stored on S3 are encrypted. Each time a save operation occurs, the editor encrypts the code into a binary file before uploading. During loading, the editor reads the encrypted code file from the cloud, decrypts it locally, and displays it in the editor.

### Data Backup

The database for published versions can be restored to any state within the last three months using MySQL binlog. For example, if data is mistakenly deleted or modified, users can contact customer service for restoration. The data recovery service incurs a fee based on the data size, with a base service fee of 500 RMB per instance. Additionally, data recovery does not include the "submitter" system field. The database for preview versions does not support restoration.

Data recovery takes approximately 5-20 minutes depending on the data volume. If the data is highly critical, users can also periodically export data to local storage for safekeeping.

### Data Recovery Flowchart

```mermaid
graph TD
A[Manual Save Click] --> B[Code Saved to S3]
B --> C[Up to 200 Versions Saved]
C --> D[Manually Save Code]
D --> E{Code Version Problem}
E --> |Load Backup Version| F[No Recovery Wait Time]

X[Publish Application] --> Y[Code Upload to Production Environment]
Y --> Z[Independent Code File Saved]

M[Database Mishandling] --> N[Contact Customer Service]
N --> O[Begin Data Recovery]
O --> P[Data Recovery 5-20 Minutes]
```

### Summary of Advantages

VisualLogic products exhibit significant advantages in data backup. Through the AWS S3 file storage service, VisualLogic applications achieve automatic multi-version backups and robust data recovery capabilities. Moreover, each backup is encrypted to ensure code security. The design's multi-region availability prevents single points of failure, ensuring high data availability and security. For critical data, users can also opt for local backups to enhance fault tolerance. Overall, VisualLogic offers users a comprehensive, reliable, and efficient data backup and recovery solution that simplifies operations while enhancing data security.
 # Hybrid Mode

## Overview

"Blend Mode" is a feature used to change the rendering mode of objects on the canvas, primarily for visible and interactive objects within the canvas. This feature provides multiple rendering mode options, allowing developers to adjust the display effects of objects in a scene according to specific needs. Below are the descriptions of several options within the blend mode:

### Rendering Mode Options

1. **NORMAL**:

   - This is the default option of the blend mode. In this mode, the object's color blends with other objects in the scene without additional processing. If the transparency is adjusted, the user can see the color of the objects below.
2. **ADD**:

   - In the ADD mode, the colors of the object and other objects in the scene are added together, resulting in a brighter overall color. This mode is often used in scenarios requiring increased brightness.
3. **MULTIPLY**:

   - In MULTIPLY mode, the color of the object is multiplied with the color of other objects in the scene, resulting in a darker final color. It is suitable for creating shadow effects.
4. **SCREEN**:

   - In SCREEN mode, the colors of the object and other objects in the scene are multiplied and then inverted, making the colors brighter. Compared to ADD mode, it is more suitable for creating glowing effects.
5. **ERASE**:

   - ERASE mode will erase the objects in the scene, revealing the canvas's background color.
6. **OVERLAY**:

   - Although no specific explanation is provided, OVERLAY mode is generally used for complex graphic effects, creating depth and shadows through layer blending.

### Notes

When applying blend modes, pay attention to the impact of the background color of the scene page on the blending effects. It is recommended to set the background color to "#FFFFFF" to more easily observe the changes in blended effects.

### Steps to Apply Blend Mode

Applying blend modes typically involves the following steps:

1. **Select Target Object**:

   - Choose the object on the canvas to which you want to apply the blend mode.
2. **Set Blend Options**:

   - In the VisualLogic IDE, find the blend mode settings panel and select an appropriate blend option (e.g., ADD, MULTIPLY).
3. **Adjust Object Properties**:

   - Adjust the object's transparency, size, position, etc., as needed to achieve the best rendering effect.
4. **Preview Effects**:

   - Preview the blend effects in the IDE to ensure the design intent is met.
5. **Refine**:

   - Make necessary adjustments based on the preview to ensure the visual effects align with expectations.

```mermaid
graph TD
    A[Select Target Object] --> B[Set Blend Options]
    B --> C[Adjust Object Properties]
    C --> D[Preview Effects]
    D --> E[Refine]
```

Through the above steps, you can flexibly apply blend modes in the VisualLogic tool to create diverse visual effects. Blend modes not only enrich graphical expression techniques but also enhance the creativity and flexibility of user interface design.
 # Difference between Preview, Release, and Published Version

### Overview

Firstly, in one sentence: The preview version and release version differ in terms of code and runtime environments, appearing the same but not being the same application. The preview version and release version differ in several aspects, while the test release version and the officially released version are completely identical at all levels, differing only in URLs. The testing release and the official release are merely a URL binding process; apart from the test release version's URL featuring an additional ?version=N parameter, everything else is identical to the official release version.

### Preview and Release Mode

VisualLogic web applications are divided into preview mode and release mode. Both modes behave identically during playback, but preview mode uses a Web Assembly engine to parse content in real-time to synchronize editor content, while release mode employs fully compiled methods for runtime performance and JavaScript code security. Therefore, in the editor, the preview application is almost real-time, whereas the release application requires some compile and bundle time.

The preview mode needs to load a relatively large code parsing engine, which may cause slower initial load times. Therefore, if you want to send a project preview to a client, it is recommended to use the release version (be it a stocked or test release version).

### Differences in Front-end Code

#### Preview Version

1. **Interpretation Mode**: The front-end code of the preview version uses an interpretation mode, having a unified code interpreter to parse the application's logic files in real-time. This allows for real-time project preview without requiring lengthy compilation for every change.
2. **Runtime Efficiency**: Due to real-time interpretation for each run, there are some efficiency losses, and the large code interpreter causes unnecessary load times and CDN traffic consumption.

#### Release Version

1. **Compilation Mode**: Upon release, the preview version is compiled into native code based on the React framework. The final released application runs identically to code directly written by programmers in JavaScript, ensuring the efficiency of front-end operations.
2. **Different from iH5**: In iH5, both preview and release versions use interpretation mode, with the release version merely copying the preview version. VisualLogic’s release version does not rely on the platform's universal interpreter, so even if platform updates cause issues with certain functions, the release version remains unaffected.

### Differences in Backend Runtime Environment/Database

#### Runtime Environment

1. **Environment Separation**: VisualLogic strictly separates the development environment (preview environment) from the production environment (release environment). This ensures the safety of online applications and prevents issues within the preview environment from affecting deployed applications.
2. **Configuration Requirements**: Applications in the release environment have higher access demands, hence the backend system in the release environment has higher configuration and stricter runtime environment checks.

#### Database

1. **Independent Deployment**: The databases for preview and release versions are physically deployed on two independent database systems. Each release synchronizes the data structure (field definitions) from the preview version to the release version, but does not synchronize the data.
2. **Table Creation**: When creating a database, only preview tables are generated. During the release, release version tables are automatically created, but initially contain no data unless manually synced from the preview version.
3. **Configuration Items**: Various database configuration information (e.g., submission limits, security protections) only apply to preview tables upon setup and only become effective for release tables after release.

### Other Backend Configuration Differences

1. **Configuration Differentiation**: Some applications may involve other configuration items (e.g., allowing opening only within WeChat). These configurations, like database settings, distinguish between preview and release versions. For instance, if an application is set to only open within WeChat in the configuration interface, the PC preview case cannot be opened. Saving these changes requires a re-release to update the configuration in the backend of the release version.
2. **Identity Verification**: Requests from the preview version's backend are unencrypted, making it possible to view transmitted and returned parameters directly in the console’s Network tab. For security, all requests from the release version are encrypted, preventing direct service debugging in the console.
3. **Statistical Services**: The release version embeds statistical services, which the preview version does not.

### Conclusion

In summary, there are differences between the preview and release versions in terms of front-end code, backend runtime environment, database, and other configurations. VisualLogic ensures the security and efficient operation of the release version through these strict distinctions and handling, while providing a stable development and preview environment. This approach offers great flexibility and development efficiency for both non-coders learning to program and skilled programmers.

```mermaid
graph TD
    A[VisualLogic Web Application Modes] -->|Interpretation Mode| B[Preview Version]
    A -->|Compilation Mode| C[Release Version]
    B --> D[Real-time Editor Content Synchronization]
    C --> E[Enhanced Runtime Performance]
    E --> F[Lengthy Compilation and Packaging Time]
    D --> G[Slower Initial Load Speed]
```

Through the detailed explanation and flowchart depiction above, users can clearly understand the specific differences and respective advantages and disadvantages of the preview version, release version, and stocked version in VisualLogic across different layers.
 # Introduction to 2D Canvas Feature

### 2D Canvas Usage Scenarios

2D canvas is widely used in the development environments of 2D games, animations, and interactive display pages. Its superior graphic rendering capability over traditional DOM elements makes it the best choice for creating smooth animations and complex interactive logic. Based on the canvas component in the browser, the 2D canvas uses the phaser.js engine (version 3.6) and provides an object-driven control mode, which is more suitable for the development of complex games compared to the traditional web environment.

### Overview of 2D Canvas Features

#### UI Elements

In addition to the commonly used text, image, and video components, the 2D canvas also provides UI elements with special functionalities:
1. **Tile Map**: Supports the arrangement of grid-like maps, setting collision properties for map tiles, and importing files exported by the Tiled map editor.
2. **Tiled Background**: Supports infinitely repeating backgrounds and background movement settings.
3. **Nine-Slice Scaling**: Supports scaling images without deforming the corners.
4. **3D Image**: Supports rotation of images on the X and Y axes.

#### Camera

The camera is an independent visual window used to display components in the current page, and it supports zooming, panning, and tracking functions, providing flexible display adjustments.

#### Animation Creation

2D canvas supports various animation creation methods:
1. **Timeline and Trajectory**: For predefined animations, allowing control over play and pause.
2. **Tweens and Tween Groups**: Define animations for state transitions and support management of multiple tween combinations.
3. **Transform States and Movement Methods**: Provide flexible animation definition methods suitable for simpler animations.

#### Physics World

Provides classic and advanced physics worlds with the following features:
1. **Environmental Gravity**: Apply gravity in horizontal or vertical directions.
2. **Object Velocity and Acceleration**: Set velocity and acceleration.
3. **Collision Detection**: Detect collisions between objects.  
The advanced physics world offers additional features such as collision boundary definitions, collision parts, and constraint component management.

#### Effects

Supports various graphic and text effects:
1. **Particle Effects**.
2. **Graphic Effects**: Such as barrel distortion, halo, and pixelation.
3. **Point Light Source**: Local illumination effects.
4. **Gesture Movements**: Implement actions such as drag and pinch-to-zoom/rotate.

### 2D Canvas Component Structure

2D canvas is a complete component system with core component structures as follows:
```mermaid
graph TB
    A["2D Canvas"] --> B["Page<br>Scene, Physics World"]
    B --> C["Container<br>Alignment Group, Texture Container, Timeline"]
    B --> D["Entity Objects<br>Images, Text, etc."]
    B --> F["Non-Entity Objects<br>Tile Maps, Particles, etc."]
    C --> D
    D --> E["Operations<br>Granularity, Gesture Control,<br>Delays, Objects, Effects"]
```
Under the root component of the 2D canvas, page-like containers can be added, including common scenes and physics worlds. Entity objects, including images, videos, text, etc., can be added to pages or containers, suitable for the main implementation of games. The page supports gesture control and effect plugins for controlling actions and interaction characteristics.

### Differences from the Older Version of the Canvas

The new version of the canvas has optimizations in both performance and functionality, including performance closer to native, more flexible physics world options, and comprehensive camera support.

### Other Features of the 2D Canvas Component

#### Canvas Resolution

The Phaser engine does not support dynamic resolution changes and must be set in advance. It is recommended to set the width to 720px on mobile devices to improve clarity.

#### Rendering Mode

Supports webgl and canvas modes. Webgl can accelerate rendering, so it is usually recommended to use the auto mode (i.e., automatically choose webgl based on support).

#### Preloading

The 2D canvas component can manage the preloading of all page components to ensure resources are presented after loading is complete.
 # Canvas Resource Preloading

### Canvas Resource Preloading

Canvas resource preloading is a technique that loads all necessary material resources in advance before a user accesses a scene page. This ensures that users do not have to wait for materials to load one by one, thereby enhancing the user experience. It is particularly important in scenarios where the materials are large or numerous. Resource preloading processes this by updating the loading progress in real-time.

#### What is Resource Preloading

Various components used in the canvas, such as images, videos, tile maps, and skeletons, rely on external material resource files. If these material resources are large or numerous, they often require a significant amount of time for downloading and rendering. Therefore, with resource preloading, users will only enter the scene page after all resources are completely loaded rather than entering the page first and then waiting for resources to load gradually.

In a 2D canvas, resource preloading is conducted on a per-page basis, and currently, there are two implementation methods: automatic preloading and manual preloading.

#### Automatic Preloading

Automatic preloading occurs during canvas initialization. When the "preload" attribute of a page is enabled, the page enters the automatic preloading process. The following are the steps of automatic preloading:

1. **Create Object Instances**: During canvas initialization, object instances in all pages are created. These objects are logical entities, such as images or videos without material. This process is very quick in memory, almost completed instantaneously. Once the object instances are created, the canvas root node will trigger an "initialization completed" event.

2. **Load Material**: At this point, the system will load the materials from pages where the "resource preloading" switch is turned on, and it will update the loading progress. If the first page has the preloading switch turned on, nothing will be displayed during the loading process. If not, the content of the first page will be displayed normally. Therefore, it is recommended to set the first page as a loading page without large materials to optimize the user experience.

3. **Complete Loading**: After the materials for all pages involved in automatic preloading are fully loaded, the page becomes accessible, and users can navigate to the target scene page.

```mermaid
graph TD;
    A(Initialize Canvas) --> B(Create Object Instances);
    B --> C(Trigger Initialization Completed Event);
    C --> D{Is First Page Preload Enabled?};
    D -- Yes --> E[Load Material Without Displaying Content];
    D -- No --> F[Display First Page Content];
    E --> G(Update Loading Progress);
    F --> G;
    G --> H(Complete Loading);
    H --> I(Navigate to Target Page);
```

#### Manual Preloading

Manual preloading suits scenarios where it is not desirable to load all pages during initialization, particularly when there are many scene pages, and loading them all at once may lead to prolonged waiting times for users. In such cases, you can perform manual preloading after initialization is completed:

1. **Disable Resource Preloading Switch**: First, disable the resource preloading switch on the pages you wish to load manually later.

2. **Initiate Manual Preloading**: Start preloading of the target page through an event (such as a button click).

3. **Load and Display Progress**: During the page's event, retrieve the loading progress to create a progress bar, and perform page navigation actions upon completion of the loading event.

This method provides greater flexibility to avoid excessively long initial loading times, offering users a more seamless browsing experience.
 # Template Import and Usage

### App Store Template

The VisualLogic App Store ([https://shop.ivx.cn/]()) provides users with a wide variety of application templates. These templates extensively cover different types, ranging from website construction and form workflows to e-commerce platforms, enterprise management systems, and city management monitoring, to meet the needs of various fields and industries. Users can search and filter to find and purchase the application systems that best suit them.

### Template Download and Import Steps

1. **Download Template**: After finding a suitable template, the system will prompt for online payment if it is a paid template. Once the payment is complete, the download can proceed; for free templates, users can download them directly.
2. **Import and Create Template Application**: After downloading, the template application will be automatically imported and created. If the import process does not complete automatically, users can manually click to create the template application.
3. **Edit Template Application**: Once successfully created, the template will automatically open in the VisualLogic editor. Users can modify and further develop the application to suit specific usage scenarios and requirements.
4. **Manage Purchased Templates**: Purchased templates can be viewed and managed in the user's personal workspace, supporting the export of templates to create applications, and allowing the deletion of templates that are no longer needed.

### Template Usage Flowchart

```mermaid
graph LR
A[Select Application Template] --> B[Click Download]
B --> C{Template Type}
C -->|Paid Template| D[Online Payment]
C -->|Free Template| E[Direct Download]
E --> F[Auto Import and Create Template Application]
D --> F
F --> G{Editor Operations}
G -->|Success| H[User Modification and Development]
H --> I[Manage Application Template]
```

Through the VisualLogic App Store and editor, users can quickly import and use application templates across various industries, making the complex application development process efficient and convenient. This method is suitable both for people who do not know code to quickly get started with application development and for experienced developers to provide a highly customizable development environment.
 # Basic Operations of the Editor

### Object Addition

In VisualLogic application development, adding objects on the stage is accomplished through the Components Panel.

1. Select the required component from the Components Panel on the left side of the editor. If a specific component cannot be found, you can search for it at the top of the Components Panel.
2. Hovering over a component displays its description; reading the component documentation before use is recommended to understand its detailed usage.
3. Clicking on a component in the Component Panel automatically adds an object of that type to the application. Under absolute positioning, after adding a visible component to the front end, you need to drag and draw the size and position of the object on the stage with the mouse.
4. The added object can not only be viewed on the stage but will also appear in the Object Tree on the right for further operations and adjustments.
5. The method for adding back-end components is the same. Clicking on a back-end component in the Component Panel automatically adds the selected component object to the application back end.

The Components Panel provides standard components and container components. When switching between different front-end and back-end applications, the Components Panel will display different types of components. For example, commonly used front-end components such as basic units, container components, form components, etc.; for the back end, there are database, service, and third-party interface components. Container components include pages, condition containers, rows, columns, etc., which can contain other entity objects.

AI models are also part of VisualLogic and can be viewed and added in the AI category of the Components Panel. Note that model runtime incurs real-time charges based on usage duration or token consumption.

### Object Adjustment

Objects added to the application generally require subsequent adjustments for deployment and use.

1. **Modify Object Name**: Objects are automatically named as "Component Category + Order of Addition," such as Image 1, Text 2. You can double-click the name in the Object Tree to enter rename mode and input a new name.
2. **Hierarchy Adjustment**: Objects initially appear in a system default position. By selecting and dragging objects in the Object Tree, you can assign objects to a specific page and adjust their relative position among same-level objects.
3. **Attribute Style Modification**: Use the Properties Panel to modify the object's attribute values to customize the object's functionality and appearance.
4. **Multi-Select Batch Operation**: Hold the Shift key and click the mouse to select multiple objects for batch settings and operations, such as position adjustment, copy-paste, and deletion.
5. **Copy and Paste**: VisualLogic supports copying, cutting, and pasting objects within the same application and cross-application copying for convenient repetitive development.
6. **Delete Operation**: Press the Delete key or select "Delete" from the context menu with the mouse to delete selected objects and their child objects.

### Adding Events

Event logic is core to realizing interactive functionality in VisualLogic development. An event includes a trigger event and a response action, for example, hiding an image component when a button component is clicked.

1. Select an object in the Object Tree and click the event icon in the Logic Toolbar on the right to add an event.
2. After clicking the event icon, the Event Panel opens automatically, allowing settings for events and actions.
3. You can establish interactive relationships by choosing component events and actions, such as assigning a value to a variable and jumping to a detail page after clicking a card.
4. To add multiple event logics, click the "+Event" button at the top.

Action groups contain multiple actions, encapsulating common logic to simplify project complexity. With action groups, you can efficiently implement calls to complex logic, which can be triggered and invoked during specific events after being added to the Logic Toolbar on the right.

### Project Save and Reversion

1. You can save the current editing state at any time by clicking the "Save" button at the top right of the menu bar or using the shortcut key (Ctrl + S).
2. The system automatically saves every five minutes. Users can load previously saved versions through the history version reversion function, and frequent manual saving is recommended to prevent progress loss.
3. Choose the "Save As" operation in the "File" menu, confirm the new application name, and decide whether to load the newly saved application.
4. User-saved history version information includes version number, save time, and save account. Through the history version function, you can view and load the required historical versions.

```mermaid
graph LR
A[Component Panel Selection] --> B[Stage Drag Position]
B --> C[Object Tree Display]
C --> D[Properties Panel Adjustment]
D --> E[Event Icon Add Event]
E --> F[Save or Reversion] 
```"
 # UI construction and layout design

### # Overview

The VisualLogic platform provides two powerful UI construction methods: one based on the "absolute positioning principle" and the other on the "relative positioning principle," assisting developers in creating UI layouts in the most suitable manner.

### # Basic Concepts of Relative and Absolute Positioning

In VisualLogic, UI component layouts primarily rely on two positioning rules: relative and absolute positioning. The choice of rule should depend on the specific requirements of the project and the desired visual effect. Each positioning method has its unique application scenarios and advantages.

1. **Relative Positioning Layout**

   Relative positioning is based on the natural document flow of HTML, where elements are automatically arranged in order, such as from top to bottom or left to right, similar to the "pipeline" in typography, allowing elements to adjust dynamically based on the size of the parent element or viewport. In this layout, setting an element's width as a percentage of the parent element's width (e.g., 50%) ensures responsive design.

   When using relative positioning, the main steps include:

   - Adding multiple buttons on a relative positioning stage, which are by default sorted from top to bottom.
   - To arrange elements horizontally, use the row component in relative positioning.
   - The "wrap" attribute of the row controls whether elements automatically wrap.

   Common properties of relative positioning layouts include alignment (such as top, center), width and height (fixed value or percentage), and margins (controlling gaps between elements), which can precisely control component layouts.

2. **Absolute Positioning Layout**

   Absolute positioning defines an element's position on the stage precisely through x, y coordinates, providing greater freedom compared to relative positioning. For instance, in UI interface design, absolute positioning is akin to marking points precisely on a map, with the top-left corner generally as the origin. With drag-and-drop functionality, users can easily adjust a component's coordinates and size.

   Configuring a component in an absolute positioning environment requires manually setting its x, y coordinates relative to its parent container. The stacking order (z-index) is used to control the overlap of components within the same parent container.

### # Applicable Scenarios for Both Positioning Rules

Relative positioning is more suitable for automatic layouts, especially advantageous for creating responsive designs. It keeps components in the document flow, making adaptive layouts easier. Absolute positioning, however, offers precise control for applications with specific interactive needs, such as animations and games.

In actual development, a combination of both positioning rules is typically used. For example, relative positioning constructs core content, complemented by absolute positioning for pop-ups or tooltips.

### # Special Layout Component: Banner

The banner component is an important positioning tool in VisualLogic, advantageous for maintaining a fixed position during page scrolling, commonly used for fixed displays of high-level visual elements like navigation bars and pop-ups.

- **Banner Characteristics:**
  - By default, banners have a high stacking order.
  - The key is the background overlay setting, providing a layered background effect.
  - Banners in VisualLogic are divided into absolute and relative positioning, embedding different positional containers.

During application development, banner components can be set for center display, semi-transparent overlays, etc. Specific applications include website top bars, navigation bars, ensuring content is always visible.

### # Custom Styles

VisualLogic supports custom component styles, providing flexible style customization methods. With custom class names (or additional element classes), developers can set component effects using CSS properties defined in style files. For example, to add a gradient background to a button:

- Select the target button and set the background style to a linear gradient color `linear-gradient(120deg, #e0c3fc 0%, #8ec5fc 100%)`.

This customization capability can extend to more complex styles like background gradients and hover color changes, bringing more creativity to UI design.

### # Summary

Through the UI construction and layout design modules of VisualLogic, developers can freely choose the positioning strategy that best suits project needs, combining special layout components and custom styles to create diverse, flexible, and attractive user interfaces. These tools not only enhance the depth and expressiveness of applications but also simplify the UI development process, making it an indispensable tool in modern development.
 # Event Panel and Logic Orchestration

## # Introduction to the Event Panel

In the VisualLogic development environment, the event panel is used to edit the interaction logic between components. Through the event panel, developers can visually orchestrate frontend event logic and backend services, enabling complex interaction logic for applications. The core concepts of interaction logic include trigger events and response actions. For instance, when a button component is clicked, make an image component hide: the button click is the trigger event, and the image hiding is the response action.

## # Basic Interaction Logic Configuration

In VisualLogic, configuring interaction logic typically involves the following steps:

1. **Add Event:** Select the object in the object tree to which you want to add an event, click the event icon in the logic toolbar to add an event, and the event panel will open.
2. **Configure Trigger Event:** In the event panel, select the trigger event from the dropdown menu (e.g., click event).
3. **Configure Response Component:** Activate selection mode and choose the response component from the object tree.
4. **Configure Response Action:** In the action dropdown menu, select the action the response component needs to execute (e.g., toggle display).

Once configured, clicking the button will trigger the toggle display effect of the image.

## # Complex Interaction and Multi-Action Configuration

In practical applications, a single event might trigger multiple response actions. By hovering the mouse and using the event panel toolbar or right-click menu, you can add new actions after the current action. It is important to note that actions in VisualLogic's event panel are executed synchronously, meaning operations are completed sequentially.

## # Execution Parameters of Actions

In most cases, component actions require input parameters. For instance, a page-turning operation needs to specify the target page number, and opening an external link requires a URL. When selecting an action with input parameters in the event panel, the input parameter box will automatically appear. The structure of parameter input will vary according to the specific action requirements. Return parameters refer to the resultant data generated after an action is executed, which can be referenced in subsequent actions.

## # Conditional Execution of Actions

Conditional blocks are used for executing conditions upon event triggering:

- **Add Condition:** Select the trigger event with the mouse and click the condition button to add a condition block.
- **Set Condition:** Within the condition block, define the criteria for executing corresponding actions when the condition is met.
- **Handle Multiple Conditions:** Multiple conditions can be nested within a condition block, supporting "AND" and "OR" relationships.

Below is a sample flowchart illustrating a simple conditional logic:

```mermaid
graph TD
    A[Trigger Event] --> B{Is Condition Met?}
    B -->|Yes| C[Execute Action 1]
    B -->|No| D[Execute Action 2]
```

### Loop Execution of Actions

In scenarios requiring repeated execution of actions, loops can be utilized:

- **Count Loop:** Set the number of executions, and actions within the loop can reference the current iteration count.
- **Conditional Loop:** Set loop conditions to execute actions when conditions are met and exit the loop when they are not.
- **Array Loop:** Traverse array elements, using the current object as the operation target for actions in each loop.

Loops can be efficiently controlled by adding termination actions within the loop body.

### Log Tracking

VisualLogic supports automatic logging in both frontend and backend, facilitating debugging for developers. Frontend logs can be viewed in the browser's console, while backend logs require enabling the log icon for additional information. These tools provide significant convenience for development and debugging.

Through the above configurations and tool combinations, VisualLogic's event panel offers developers flexible and powerful logic orchestration capabilities, simplifying the implementation of complex interaction logic.
 # Backend Data Design

## Backend Data Design

In a VisualLogic application, developers can build and design backend data through a visual interface, primarily relying on database components and data view components. Additionally, VisualLogic supports connecting external databases via the DBO component to facilitate external data management and access. These features allow developers to easily integrate multiple data sources and develop feature-rich applications.

### Database Panel

Databases are one of the fundamental components for storing application persistent data. In VisualLogic, existing databases use traditional relational database structures, specifically the MySQL system. Each database created corresponds to a MySQL data table in the backend. Databases in VisualLogic are divided into application databases, group databases, and account (enterprise) databases:

- **Application Database**: Only available within the current application. To share with other applications, microservices or public services must be used.
- **Group Database**: Usable across all applications within the same group, but to share data with applications outside the group, public services are still required.
- **Account (Enterprise) Database**: Shareable within the current account or enterprise; to share with external systems, public services must be used as well.

#### Database Creation Steps

1. **Add Database**: Add a database to the application from the backend components. After opening the database panel, click the [+Field] button to add necessary fields.
2. **Preview Data**: After adding fields, the preview data table will display data in a tabular format. Each column represents a field, and rows represent each database record. Data can be manually added to the preview table for testing.
3. **Publish Data Table**: Use the data table after application release. After publishing, the structure of the published table remains consistent with the preview version, but data will not be synchronized. To synchronize, manually click the [Sync Preview Data] button.
4. **Quick Import of Fields and Data**: VisualLogic editor provides a feature to quickly import fields and data from an EXCEL file. First, prepare an EXCEL file with the structured standard and test data, then click the [Import] button in the database panel, select the Excel file, and the data can be imported.
5. **Field Management**: The database panel supports operations such as field modification, comments, index settings, and data export.

### Data View

#### Single Table View

Database views essentially store the mapping and linking relationships of databases. When used as a single table, it can be regarded as a virtual database that supports CRUD operations. Meanwhile, VisualLogic's database views support the visual design of backend data structures. Key operations include:

1. **Add Database View**: Add to applications from the backend components. In the view panel, there is a separate field setting interface; click [+Add Field] to add a new field.
2. **Create and Bind Database**: After field settings, click the [Create DB and Bind] button. The system will automatically create a database matching the field names and types and bind it as the data source.
3. **High Reusability**: Database views can be reused across applications. After copying to other applications, use the [Create DB and Bind] button to quickly build similar databases and easily link with other data tables.

#### Multi-Table View

In VisualLogic, multi-table views are used to address scenarios that require multi-table joins. When used as multi-tables, views can be understood as a visual editing interface for join operations, enabling union queries and statistical outputs across multiple tables.

1. **Add New View Table**: Click the [+Add] button on the left of the view properties panel to add a new view table. Existing databases can be bound, or fields can be manually added before creating a database.
2. **Establish Connection Relationships**: Create multi-table connection relationships through links on the left side of the table. The default is left join, and join methods can be switched. For instance, establishing connections between the book information table, purchase order table, and employee table in a book procurement data view.
3. **Preview Multi-Table Data**: In the data preview table of the view, data from three sources will be merged and displayed under the connection relationships. Multi-table views allow users to query and output combined table data, operating similarly to regular databases.

### DBO: Connecting External Databases

DBO is a tool in VisualLogic designed to connect custom databases. It currently supports connections to MySQL, Oracle, SQL Server, and Postgres databases deployed anywhere. Within the service logic of VisualLogic, users can write SQL to operate databases and obtain return results, achieving service integration.

#### Application Scenarios for DBO Component

- **Connect External Databases**: Enables data querying and operations on external databases, eliminating the hassle of data migration.
- **Cross-System Data Integration**: Supports simultaneous access to multiple different database systems to generate comprehensive reports by combining data.
- **Complex Queries and Transaction Processing**: Ensures consistency in order and inventory management in applications like e-commerce platforms.
- **Legacy System Compatibility**: Adds new functionalities to old systems without extensive revamps.

### Example of Common Application Database Structures

Illustrating a common application database structure, even for a simple application like an e-commerce platform, requires designing multiple database tables to meet various functional requirements. A typical e-commerce application database structure might include the following tables:

1. **Users Table**: Stores basic user information.
   - Fields: User ID, Username, Password, Email, Registration Time, etc.
2. **Products Table**: Stores product information.
   - Fields: Product ID, Product Name, Description, Price, Stock Quantity, Category, etc.
3. **Orders Table**: Records order information.
   - Fields: Order ID, User ID, Order Time, Status, Product List, etc.
4. **OrderDetails Table**: Details each order’s product information.
   - Fields: Order ID, Product ID, Quantity, Unit Price, etc.
5. **Categories Table**: Stores product category information.
   - Fields: Category ID, Category Name, Parent Category ID, etc.
6. **Cart Table**: Records products added to the shopping cart by users.
   - Fields: User ID, Product ID, Added Time, Quantity, etc.

Building such database structures can assist in developing an e-commerce application that meets business needs and provides efficient user experiences. With VisualLogic's database panel and view functions, these databases can be quickly constructed and managed, enhancing development efficiency.

This comprehensive technical description of VisualLogic's backend data design helps in understanding how to utilize VisualLogic's database and view features to build a robust and flexible backend data system.
 # Service Development and Debugging

### Overview

In VisualLogic, services function as the core components enabling data interaction between front-end applications and back-end databases. The mechanism operates through front-end requests; once the service is initiated, various processing demands (such as CRUD operations) are conveyed to the back-end, and the results are returned to the front-end as parameters. Front-end applications can retrieve return parameters and execution states through service callbacks.

Services can be likened to restaurant waiters, with front-end applications as the ordering customers and the back-end database as the kitchen. Services record customer orders, relay them to the kitchen, and deliver dishes to the customer's table. This architecture isolates direct interaction between front-end and back-end, enhancing data exchange security and ensuring monitorable interaction statuses.

### Basic Usage of Services

#### 1. Adding Services

Firstly, services are back-end components that need to be added in the back-end directory of the application:

- Select the application's back-end, click the service icon to add it to the application, and assign a suitable name, such as "Query Phone."

#### 2. Setting Parameters

Although some scenarios allow for the use of services without setting parameters to accomplish most tasks, in most cases, service calls require setting input and output parameters:

- Click on the service component; in the parameter panel, click the plus "+" sign to create "Input Parameters" and "Output Parameters," naming them "Receive Name" and "Return Phone" respectively.

#### 3. Logic Orchestration

The logic orchestration of back-end services is similar to event logic orchestration:

- Write the logic of database query output into the service body, using "Receive Name" to filter the "Name" field.
- In the next step, add an action block to assign a value to "Return Phone" using "Current Service - Set Return Value," ultimately returning the phone number.

#### 4. Service Debugging

- Return to the input parameters, input "Zhang San" as the debugging value, and click the [Debug] button to check the service correctness through debugging results.

#### 5. Service Invocation

- Both front-end and back-end can invoke service results, avoiding nested service usage in the back-end.
- For example, the service can be invoked through a query button click event, with input box content as input parameters and output parameters displayed in the text.

```mermaid
graph TD;
    A[Add Service] --> B[Set Parameters]
    B --> C[Orchestrate Logic]
    C --> D[Service Debugging]
    D --> E[Service Invocation]
```

### Service Sharing and API

- Enable the "Set as Public Service" switch on the service panel to publish the service as an API, generating an API address. Users can customize the path name.
- Under the "Set as Microservice (Enterprise)" or "Set as Microservice (Group)" switch, services can be published as microservices for enterprise or group sharing.
- Created microservices can be viewed in the back-end resources pop-up and added to the application back-end using the [Add] button.

### Service Execution and Concurrency

Actions within a service execute synchronously, meaning each action completes before the next operation commences. VisualLogic uses Go's Goroutine for parallel execution of services, demonstrating excellent concurrency performance.

VisualLogic platform applications have the following concurrency limits to ensure access security and control back-end costs:

- The limit for new application openings is 1000 times per minute; if exceeded, new users will see a "Service Busy" page.
- A maximum of 100 service requests is processed per second, with additional requests returning "Service Busy" messages.

After private deployment of applications, developers can adjust concurrency limits independently. To increase concurrency limits on the VisualLogic public platform, please contact customer support.

### Differences Between Services and Action Groups

- **Position of Addition**: Services can only be added to the back-end, whereas action groups can be added to both front-end and back-end.
- **Primary Functions**:

  - Services mainly handle data interactions between front-end and back-end and backend logic processing.
  - Action groups bundle multiple actions for easier reuse and management.

- **Use Cases**:

  - Services are suitable for operations requiring back-end communication.
  - Action groups are ideal for packaging logically repetitive operations.
 # Encapsulation and Reusability

## Encapsulation and Reuse

In VisualLogic, code snippets are encapsulation units used for frontend, backend, or full-stack functionalities of an application. Through encapsulation, code snippets can be reused across multiple applications and exported as Vue and React components for further development and integration. The use of code snippets in VisualLogic 5.0 is similar to the small modules in version 4.0, with core functions including logic reuse, module decoupling, and real-time preview of frontend functionalities.

### Working Principle of Code Snippets

At the core, code snippets encapsulate a series of objects from the object tree into a single component. This encapsulation creates an independent component space that completely isolates interactions within and outside of it. For instance, before encapsulation, internal objects might bind to properties of external objects. Post-encapsulation, these internal and external objects cannot directly interact because all internal objects are wrapped in a "component shell," and external objects can only affect the shell's properties or respond to its events. For users, an encapsulated code snippet is just a regular VisualLogic component, not requiring a deep understanding of its internal structure, enhancing the component's maintainability and reusability.

### Steps to Encapsulate Code Snippets

1. Select one or more objects within the application.
2. Right-click and choose "Encapsulate as Code Snippet," which will encapsulate the selected objects and their child objects into a code snippet.
3. A new code snippet structure is generated in the object tree, including the code snippet root, external interface container, and the original object structure selected during encapsulation.
4. Handle the internal and external interactions during encapsulation, ensuring all interactions are legitimate. Make manual adjustments if necessary to ensure the correct encapsulation of the code snippet.

```mermaid
graph TB
  A[Select Objects] --> B[Encapsulate as Code Snippet]
  B --> C[Generate Code Snippet Structure]
  C --> D[Handle Internal/External Interactions]
  C --> E[Encapsulation Success]
  D --> E
```

### Attributes of Code Snippets

Frontend code snippet attributes are used to control the snippet's behavior or pass data within it. For example, you can create an attribute to uniformly control the language of text inside the snippet or manage a list of menu options. Attributes are set in a unidirectional manner, meaning they only flow from the code snippet attributes to internal objects and not vice versa. If internal data needs to feedback outward, an event interface container must be used for data transmission.

### Actions within Code Snippets

Code snippets can define their own actions by adding action groups through the action interface container. These action groups become methods callable by the snippet. For example, adding a "blink" action group to make text blink when called. It's important to note that action groups can only manipulate objects within the snippet and cannot directly operate on external objects.

### Events of Code Snippets

Code snippet events are used to transmit results of internal operations to the outside or to trigger external responses internally. For instance, when an internal button is clicked or an API returns a result, information is transmitted externally through a custom event component. These events need to be pre-defined in the event interface container and can carry multiple parameters.

### Uploading and Downloading Code Snippets

Once created, code snippets can be uploaded to the cloud, assigned categories, and have version notes added, facilitating reuse and sharing across different projects. The upload process automatically manages version numbers, allowing version comparisons and usage of the latest version during updates.

### Service Development and Debugging

Services in VisualLogic are used to facilitate data interactions between frontend applications and backend databases, serving as a key means of logic reuse. Frontend applications can initiate service requests, with services transmitting corresponding operation instructions to the backend and returning the processed results. For example, you can view the frontend application as customers in a restaurant and the backend database as the kitchen, while services act as the waiters who convey the customers' orders to the kitchen. The use of services enhances system security and maintainability.

The encapsulated code snippets can be seen as developer-created components, which can be uploaded, shared, exported, and used for secondary development, significantly enhancing the development flexibility and usability of VisualLogic. Through encapsulation and reuse of code snippets, developers can not only develop applications more efficiently but also ensure the readability and maintainability of the code. For more detailed information on code snippets, refer to the component manual -> Custom Components -> Creating Modules and Components in IDE -> [5.0] Code Snippets.
 # Fast development with VisualLogic

#### VisualLogic Development Efficiency Improvement

VisualLogic is a general graphical programming language and a complete integrated development environment (IDE) designed to accelerate application development by lowering learning barriers and simplifying the development process. The reasons for its enhanced development efficiency can be analyzed from the perspective of different types of developers.

##### Users without coding experience

- **Low learning curve**: VisualLogic offers a friendly learning curve, especially for those with good logical thinking but no programming background. VisualLogic allows users to avoid memorizing and understanding code syntax through graphical logic arrangement, enabling them to develop complex applications without learning code.
- **No need to memorize code structures**: VisualLogic uses easy-to-understand language and visual components to replace traditional programming structural elements, such as conditionals and loops. This greatly reduces the difficulty of learning and development.
- **Learning code in reverse**: Users can understand the logical structure of applications through VisualLogic and then reverse engineer the code implementation.
- **Real-time feedback and documentation**: The editor provides detailed documentation and real-time error prompts, allowing users to easily obtain support and solve problems during editing.
- **What you see is what you get**: Users can immediately see the components they add and their effects, reducing development anxiety.

##### Users with coding experience

- **No need to set up a development environment**: Developers don't need to establish complex development environments; they can start developing by accessing the VisualLogic IDE over the network.
- **Unified development experience**: VisualLogic integrates the front-end and back-end into one editor, improving data read and write efficiency and supporting unified front-end and back-end management and development.
- **Efficient logic arrangement**: Logic arrangement between components is done through an event panel, making mouse operations efficient and avoiding syntax errors.

#### Componentization and Information Abstraction

- **Component-oriented programming**: VisualLogic encapsulates many proven solutions into components, greatly reducing the writing of redundant code and complex logic.
- **Efficient information generation**: VisualLogic provides a high level of abstraction for encapsulation and reuse, making code generation extremely efficient, allowing for substantial code generation through component calls in a unit of time.
- **Component and module reuse**: By reusing components and modules, repetitive work is reduced, speeding up the development process.

#### Process Optimization Example

The efficient development process of VisualLogic is as follows:

1. **Preparation before development**: No need to set up a specific development environment.
2. **Component addition**: Drag and drop to add required front-end and back-end components.
3. **Logic arrangement**: Configure logic in the event panel and implement complex logic through simple mouse operations.
4. **Testing and editing**: Use the IDE's real-time feedback feature to test while developing.
5. **Application release**: An intuitive and step-by-step release process ensures the application is error-free.
// Use Mermaid to visualize flow
```mermaid
graph LR
    A[Preparation before development] --> B[Component addition]
    B --> C[Logic arrangement]
    C --> D[Testing and editing]
    D --> E[Application release]
```

#### Auxiliary Functions and Resource Management

- **High-efficiency collaboration**: VisualLogic supports multi-person development and rigorous version management, enabling quick responses to user feedback through name search and regular iterations.
- **Resource management**: It supports front-end and back-end resource management, automatically handling and importing materials to improve development efficiency.
- **Application transfer and update**: Applications can be quickly transferred to new maintainers via account or application handover, facilitating convenient maintenance.

The overall goal of VisualLogic is to provide developers with an efficient, low-threshold development method through the optimization of underlying technologies and product design, allowing them to learn, develop, and operate applications more quickly. Compared to traditional programming, VisualLogic not only speeds up development but also eliminates complexities arising from diverse technology stacks.
 # The optimal choice for programming learners.

##### Reasons to Learn VisualLogic Programming

With the development of modern technology, programming has increasingly become an indispensable skill in life and work. VisualLogic is an excellent choice for learning programming because it allows users to implement application logic through graphical components without learning complex programming languages. VisualLogic is a language designed for the "era of programming for everyone," aligns with the STEM education trend, and is suitable for those transitioning to the internet industry, students, and existing developer groups.

##### Architecture of the VisualLogic IDE

The integrated development environment (IDE) of VisualLogic consists of several important panels: component panel, properties panel, object tree panel, event panel, data flow panel, navigation bar, menu bar, stage auxiliary configuration bar, logic component bar. These panels enable users to connect and configure components in a visual manner, thus realizing the complete logic of an application.

Any component added in the IDE will be displayed in the object tree panel, becoming part of the application. Component design is based on object-oriented design patterns, including trigger conditions, properties, and functions. This entire design approach makes VisualLogic Turing-complete in application logic, similar to function composition in programming.

##### Advantages of VisualLogic for Different Users

1. **For Learners**:
   VisualLogic is suitable for those with logical thinking but no coding experience, even requiring only an understanding of Mandarin. Learners can master full-stack development capabilities within weeks. VisualLogic's syntax-free programming approach simplifies the learning process, making it one of the easiest programming languages to pick up. Additionally, VisualLogic supports the generation of a wide range of applications (such as web applications, mini-programs, desktop applications, etc.), making its usage scenarios very broad.
2. **For Programmers**:
   As the latest graphical programming language, VisualLogic not only possesses frontend and backend development capabilities but can also automatically generate high-quality full-stack code. Programmers can embed code through custom components, custom functions, and other methods to supplement and expand application functionality.
3. **For Enterprise Users**:
   VisualLogic can serve as a rapid code generator and enterprise development platform, providing efficient and secure development and operations solutions. VisualLogic includes a complete set of enterprise-level operations environments and solutions, such as logging systems, message management, K8s+Docker resource management, etc., facilitating enterprises to build a middle platform and manage data assets.

##### Comparison of VisualLogic and Traditional Programming

Compared to traditional programming languages, VisualLogic is easier to learn, adopting "component-oriented programming" to lower the learning curve. This approach enhances development efficiency, allowing developers to almost exclusively focus on business logic implementation without worrying about complex code syntax and environment configuration. VisualLogic, through its powerful component and event panels, can generate multiple lines of code at once, significantly reducing the probability of bugs.

VisualLogic not only smooths out differences between different programming languages but also serves as an adhesive for existing code, supporting high integration and maximization of code and development assets.

##### Why VisualLogic is the First Choice for Programming Learners

Through its visual programming language, VisualLogic contrasts with complex high-level languages, significantly lowering the threshold for learning programming. At the same time, by providing automatic code generation and comprehensive development support, it compresses knowledge that would take years to master into weeks, greatly enhancing learning and development efficiency. VisualLogic can not only develop new applications but also serve as a solution for integrating and communicating application data.
 # The Best Assistant for Programmers

VisualLogic, as a graphical programming language, is not only friendly to traditional code programmers but also offers convenience to coding learners who do not understand code.

##### 1. VisualLogic is a programmer-friendly programming language

Since the birth of programming languages, the pursuit of language universality and simplicity has been the goal of many companies and institutions. VisualLogic has made groundbreaking progress in this area, achieving a unification of "completeness" and "no-code programming". VisualLogic can be seen as a front-end and back-end code generator, with flexible support for cloud computing resources, making it particularly friendly to programmers. On the front end, VisualLogic can generate React visual code, while on the back end, it can generate business logic code and SQL syntax. For cloud resources, VisualLogic equates them to a combination of BaaS and FaaS services, helping application developers seamlessly support elastic IaaS layer resources.

##### 2. VisualLogic's product features and developer support

VisualLogic is not just a code generator but also an effective development tool that provides complete support for development, testing, deployment, and operation. Developers can use VisualLogic to develop applications ranging from simple to complex, with multi-platform support for both web and local operation. Additionally, VisualLogic is committed to maintaining the logical properties of code, retaining the logical structure of problem-solving while removing syntax, which significantly enhances the development efficiency of existing code programmers.

##### 3. VisualLogic's code inclusion

In maintaining openness, VisualLogic allows programmers to embed their code in multiple modules, including support for custom front-end components, custom functions, JSON operations, and more. For database support, VisualLogic supports automatic generation of syntax programs for databases like MySQL through AST and supports SQL syntax management for other databases via the DBO method.

##### 4. VisualLogic's market and target customers

VisualLogic is targeted not only at individual developers but also at enterprise users. For individual developers, VisualLogic offers a free IDE version that supports a complete application development environment, combined with a rich component library and online marketplace, enabling developers to conveniently develop applications. For enterprise customers, VisualLogic not only supports efficient development but also provides enterprise-level development management and security features, including permission management and R&D content management. VisualLogic has multiple advantages, being easier to learn than traditional high-level languages, offering higher development efficiency, and meeting the continuous operation and maintenance needs of enterprises.

##### 5. Technical pain points and innovations solved by VisualLogic

Traditional programming languages often require developers to master cumbersome syntax and various frameworks, and application development and operation often involve high handover and management costs. VisualLogic, through graphical logic editing and no-code programming paradigm, significantly lowers the learning threshold and development difficulty. Its innovative graphical programming method not only enhances the speed and maintainability of development but also allows free embedding of code, thus meeting the needs of various developers and enterprises.

In summary, VisualLogic provides developers with an efficient, flexible, and multifunctional development platform. Through its graphical programming language and comprehensive component support, it significantly boosts development efficiency while reducing the coding burden, becoming the best assistant for programmers in application development.
 # How to Recruit and Train VisualLogic Engineers?

In recent years, with the gradual popularization of VisualLogic technology, enterprises and individuals are actively seeking and cultivating VisualLogic engineers. VisualLogic is a new graphical programming language that focuses on component-based programming, significantly enhancing development efficiency and allowing those without coding knowledge to participate in programming.

#### Hiring VisualLogic Engineers

When hiring VisualLogic engineers, several aspects need to be considered:

- **Hiring Suitable Candidates**: Ideal candidates for VisualLogic development include graduates with a degree in computer science or related fields, frontend developers with 0-3 years of experience, and junior programmers looking to transition. Candidates with strong logic skills, some mathematical modeling ability, or experience in programming competitions (such as ACM) are particularly suitable. Additionally, those looking to transition, despite requiring more resources for training, have a higher likelihood of learning VisualLogic at a relatively lower cost.
- **Avoid Hiring High-Salary Backend Programmers**: Since VisualLogic to some extent "levels" the playing field for backend programmers, those with high salaries and a focus on traditional backend development are not well-suited for the VisualLogic environment.
- **Choosing Recruitment Channels**: Recommended platforms include Lagou, Boss Zhipin, Liepin, and leveraging campus recruitment to attract newcomers, as fresh graduates generally have strong learning abilities and can quickly adapt to VisualLogic development.
- **Interview and Selection Methods**: During the selection process, it's suggested to arrange learning tasks through VisualLogic.cn to observe candidates' interest and self-learning ability, such as modifying assignments on Demo.ivx.cn. In addition, written and interview questions can test logical abilities and algorithmic thinking. Ensure candidates can adapt to visual development methods and possess solid logical thinking.

#### Training VisualLogic Engineers

Once successfully recruited, a reasonable training plan is essential:

- **Continual Learning of New Languages**: VisualLogic is a new type of development language, and learners should focus on mastering core content such as component interfaces, event management, and data interaction.
- **Utilizing Online Learning Resources**: With the help of VisualLogic's official teaching resources, community support, and case studies, engineers can quickly get started and gain an in-depth understanding of the entire VisualLogic development process.
- **Emphasizing Practical Experience and Extensibility**: New employees should participate in real projects to rapidly enhance their practical development abilities through ongoing training and real development experience.
- **Fostering the Combination of Logic and Creativity**: VisualLogic's graphical logic arrangement helps cultivate employees' logical thinking and creativity to adapt to technological changes.

#### Recruitment and Training Flowchart

Below is the basic process for recruiting VisualLogic engineers:

```mermaid
flowchart LR
    A[Determine Recruitment Needs] --> B[Select Recruitment Platform]
    B --> C[Post Job Information]
    C --> D[Screen Resumes]
    D --> E[Arrange Initial Interview]
    E --> F[Arrange Interview]
    F --> G[Offer Notice]
    G --> H[Formulate Onboarding Training Plan]
    H --> I[Arrange Project Practice]
    I --> J[Continuous Learning and Development]
```

With the further maturity and extensive application of VisualLogic technology, VisualLogic engineers will become an indispensable backbone in the information technology field. Enterprises need to actively adapt to this technological transformation by establishing efficient VisualLogic teams through effective recruitment and training strategies.
 # Enterprise Account and Services

#### Business Account Functions and Value

The VisualLogic business account is offered as a paid service and is currently the main source of income for VisualLogic. Therefore, business users need to pay a certain fee to access comprehensive account features and services. VisualLogic provides a highly flexible development environment that supports efficient enterprise application development, costing only 4880 RMB per developer account annually, yet achieving higher efficiency and effectiveness than ordinary development teams. Through centralized management and security measures, business accounts ensure the safety of application code and development data, reducing risks and costs in enterprise development.

The business account supports multi-person collaboration, requiring at least a primary and subsidiary account model, which helps manage enterprise teams and distinguish personal accounts. Compared to the personal version, the business version includes more enterprise-level development features, such as enterprise databases, microservices, and connectivity, along with powerful version management support.

For educational institutions, VisualLogic offers free educational accounts for academic purposes, but these do not support commercial development. The growth of published applications is relatively complex, leading to higher cross-platform migration risks, hence we offer continuous deployment and operational support.

#### VisualLogic Service Types

VisualLogic has tailored a variety of services for enterprises, mainly divided into two categories:

1. **Public Cloud VisualLogic IDE Account Authorization Service**:

   - Similar to the SaaS model, charges are based on the number of enterprise developer accounts, with yearly or monthly payment options. Business accounts can conveniently manage digital assets, prevent data leaks, support sub-account permission allocation, and provide detailed personnel work report viewing. This service is particularly suitable for small and medium-sized development teams, startups, and organizations with limited research and development capabilities.
   
2. **Private Deployment**:

   - Enterprises deploy the full VisualLogic system internally, which offers a higher security level compared to the public cloud model, suitable for enterprises with high information sensitivity and strong IT capabilities. Enterprises can fully control network and server resources, ensuring no data or applications are leaked.

Additionally, VisualLogic provides a standard enterprise-level runtime environment based on Java Spring Cloud, supporting CI/CD and DevOps to help enterprises maintain efficient development and operational processes.

#### Application System Deployment Practice

The VisualLogic enterprise application system provides phased deployment and management, offering development, testing, and production environment isolation support, with each environment corresponding to an independent K8s cluster:

1. **Application Development and Deployment Architecture**:

   - Implements request distribution via load balancing or cloud gateways.
   - Uses Alibaba Cloud Nacos as the service registry center.
   - Achieves comprehensive cloud resourceization of applications, microservices, files, databases, caches, and message queues, reducing the complexity of managing own facilities.
   
2. **Deployment Management Process**:

   - Configures application repositories for deployment service address management and sets up repository administrators.
   - Once application development is completed, the deployment package is pushed via the VisualLogic editor to perform Java source code compilation, Docker image generation, and other operations.
   - Deployment offers the choice between testing or production environments, with options to optimize the system by adjusting environment variables and container specifications.

3. **Update and Rollback Features**:

   - Supports one-click deployment and the selection of different versions for online switching, with immediate rollback capabilities for stability.

4. **Logging and Monitoring**:

   - Provides integration of local and cloud service interfaces for log access, supports dynamic monitoring, and ensures timely response to system anomalies.

5. **User and Data Management**:

   - Employs a user center for user authentication and permission configuration.
   - Implements multi-tenancy and data isolation to ensure complete resource independence among different organizations.

Mermaid flowchart description of VisualLogic enterprise application development and deployment process:

```mermaid
graph TD
    A[Access VisualLogic Work Platform] --> B[Configure Application Repository]
    B --> C[Push Application Image Package]
    C --> D[Select Deployment Environment]
    D --> E[Adjust Environment Variables and Resource Configuration]
    E --> F[One-Click Deploy Application]
    F --> G[Monitor Logs and Application Status]
    G --> H{Application Update and Rollback}
    H -->|Select New Version| F
    H -->|Retain Current Version| G
```

Through these features and services, VisualLogic not only provides enterprises with a stable development and application hosting environment but also significantly reduces enterprise IT operation management costs, enabling enterprises to focus on their business development and innovation.
 # Team and Performance Management

### VisualLogic Enterprise Team and Performance Management

In modern enterprises, the effectiveness of project management often determines whether a project can succeed. The VisualLogic platform, with its unique tools and methods, enhances enterprise team and performance management, significantly improving the efficiency and effectiveness of project management.

#### Improvement in Project Management

VisualLogic's understanding of project management and enhancement can be summarized as "Tools as Language, Logic as Code, Product as Architecture, R&D as Operations." This unique approach simplifies the complex project management process and, through an integrated development environment and component-oriented programming, brings about a more efficient management process.

- **Tools as Language**
  VisualLogic combines "Tools IDE" and "Programming Language." Although the programming language is invisible to the user, through the IDE development tool interface provided by VisualLogic, developers can generate code automatically through tool operations. This combination greatly reduces the development difficulty and enhances development efficiency.
- **Logic as Code**
  In VisualLogic, logic is expressed through an "Event Panel," which naturally generates code snippets. This fully Chinese event logic is easy to understand, allowing even those without a development background to participate in the development process easily.
- **Product as Architecture**
  With the introduction of VisualLogic, the role of product managers in the team is strengthened, allowing them to almost take on the work of an architect in product development. By learning VisualLogic, product managers can deeply engage with product foundations, manage architecture work comprehensively, decide on the number of database tables used, and provide necessary services and interfaces. This symmetrical and transparent knowledge environment promotes effective internal communication and improves product performance and experience.

#### Advantages of VisualLogic Products

The advantages of VisualLogic are reflected in its powerful graphical programming language IDE. Through component-oriented programming and graphical logic arrangement, VisualLogic supports collaboration on the same platform from learners who do not understand code to professional programmers. The developed application's front-end can choose to generate code in Vue or React frameworks, while the back-end supports Java SpringBoot framework code. These features of VisualLogic allow enterprise teams to complete complex project management with lower learning costs and higher development efficiency.

- **Rapid Learning and Development**
  VisualLogic, with its extensive visual components and intuitive logical representation, makes learning more rapid and understandable. This not only reduces reliance on programming knowledge but also enhances development speed. The entire process supports no-code development while generating high-quality code, providing a good cooperative environment for both non-technical and technical personnel.
- **Comprehensive IDE Structure**
  By including components panel, properties panel, object tree panel, event panel, and data flow panel, VisualLogic provides a complete development toolset. Each component is object-oriented encapsulated, easy to develop and maintain.
- **Developer-Friendly**
  VisualLogic supports directly embedding code into the front and back ends, supporting various programming languages such as Java, Python, Node.js, Android SDK uploads, handwritten SQL, CSS, HTML code, and npm package imports. This high flexibility allows professional developers to apply their specialized skills freely.

### Summary

Through its unique development tools and methods, VisualLogic provides an efficient solution for enterprise team and performance management. With the philosophy of Tools as Language, Logic as Code, and Product as Architecture, VisualLogic not only improves the efficiency and transparency of project management but also enhances team collaboration capabilities, ultimately playing a crucial role in project success. By introducing VisualLogic, enterprises can respond quickly to customer demands in a highly competitive market, maintaining a leading edge.
 # VisualLogic cooperation plan with colleges and training institutions

## Cooperation Background

1. Party B is a globally leading company in visual development and no-code development technology, focusing on the research, promotion, and application of visual and no-code compilation technology for over ten years, with 50 international and national patents. Party B's iH5 Cloud Creation Platform and VisualLogic no-code development platform have nearly 20 million registered users and more than 500 enterprise users, receiving positive market feedback.
2. Party A agrees to introduce Party B's training courses to promote the popularization of programming development thinking, theory, and technology, thereby enhancing students' innovation, entrepreneurship, and employability.

## Course Offering

1. Both parties will offer programming design and production courses based on the iH5 Cloud Creation Platform and VisualLogic no-code development platform in relevant departments or majors at Party A’s institution.
2. Party B will provide Party A’s teaching staff with one free iH5 Cloud Creation Platform or VisualLogic no-code development platform teaching account and corresponding student accounts. Party B will also provide complete teaching plans, teaching cases, teaching videos, and case materials free of charge.
3. During the cooperation period, Party B will provide free online technical support to Party A, answering technical questions.
4. Party A shall provide the necessary teaching staff and teaching environment for the course offering.
5. The cooperation period is one semester, from March 1, 2021, to August 31, 2021.

## Teaching Account Activation and Usage Instructions

1. The usage duration for the teaching account is half a school year/one semester. Party A must apply for account activation or renewal before the start of the semester and provide the necessary materials.
   - For initial activation, provide the teacher's edu email, proof of qualifications related to the course, teaching plan, and scale.
   - For renewal, provide teaching account information, proof of course offering, latest teaching enrollment numbers, and the number of newly opened sub-accounts.
   - The teaching account is provided to Party A for free, but any incurred costs must be borne by Party A.
   - Special circumstances can be discussed separately.
2. From the activation date, each sub-account must remain active (with more than ten creations) within three months, or else Party B has the right to revoke access.
3. The teaching account may not be resold or used outside the contracting departments and is prohibited from commercial use. If commercial activities are found under the account, Party B has the right to revoke access.
4. Party A must properly manage the account and password to prevent leakage. In case of anomalies, Party B should be promptly notified.

## Intellectual Property

1. The intellectual property related to products, technology, and services of the iH5 Cloud Creation Platform or VisualLogic no-code development platform belongs to Party B.
2. During the agreement period, applications or research results independently completed by Party A based on the relevant platform technology belong to Party A.

## Confidentiality Clause

1. Both parties agree to keep all confidential information (e.g., pricing, discounts, information, computer confidential data, etc.) under the agreement confidential to prevent leakage.
2. Confidential information must not be disclosed to third parties without consent unless required by law.
3. The confidentiality clause remains effective even after the termination of the agreement.

## Other Provisions

1. The agreement becomes effective upon signing by the authorized representatives of both parties and affixing the official seals.
2. Unspecified matters can be supplemented by mutual agreement to form an effective agreement.
3. Disputes should be resolved amicably through consultation. If unsuccessful, they may be brought to court.
4. The agreement is made in two copies, with each party holding one copy, both having equal legal effect.
 # Common Issues and Answers

### 1. VisualLogic Platform Overview

#### 1. What is VisualLogic?

VisualLogic is a comprehensive visual logic editing solution designed to organize various logics in a graphical manner, ultimately solving problems by constructing various "models" through "logic links." VisualLogic abstracts and structures traditional programming languages to achieve a lower learning curve, faster development efficiency, and easier operations and maintenance.

#### 2. What are the core technologies and working principles of VisualLogic?

The core technology of VisualLogic is "Component-Oriented Programming (COP)," which encapsulates code into components, allowing developers to build applications like building blocks. The working principle of VisualLogic is divided into three steps:

- Abstract all componentizable entities into components, each containing trigger conditions, properties, and functions/methods.
- Express logic through graphical means via the event panel and data flow panel without writing code.
- Combine components and logical expressions to finally generate frontend (Vue/React) and backend (Java) code.

#### 3. What are the advantages of VisualLogic?

- **Fast Learning**: No need to learn complex syntax and frameworks, just grasp the use of components and logic orchestration.
- **Fast Development**: Each operation on average generates over 300 lines of code, significantly shortening the development cycle.
- **Fast Runtime**: The generated code is simple and efficient, ensuring high runtime efficiency.
- **Fast Maintenance**: Supports CI/CD and DevOps, providing a complete enterprise-grade maintenance environment.
- **High Security**: Component mechanism isolates code, encrypts communications, integrates with cloud platforms, and supports code encryption and scanning.
- **Strong Maintainability**: The generated code is highly readable, easy to maintain, and conducive to secondary development.

#### 4. What is the essence of VisualLogic?

VisualLogic is essentially a comprehensive visual logic editing solution, providing rich, customizable components, and using the event panel and data flow panel to achieve rapid application logic expression, ultimately generating executable front-end and back-end code.

#### 5. What is the design philosophy of VisualLogic?

The goal of VisualLogic is to design a complete "Visual Logic" solution. By organizing the "world" with "visual logic," it allows everyone's creativity and ideas to be expressed through "visual logic," eventually forming code and becoming applications or programs.

### 2. VisualLogic Products and Features

#### 6. What are the basic components of VisualLogic IDE, and their functions?

The VisualLogic IDE consists of five main parts: component panel, object tree, component property panel, event panel, and stage. These are used respectively to manage components, organize component hierarchy, configure component properties, edit application logic, and preview application effects.

#### 7. What components are included in VisualLogic?

VisualLogic includes visual components, container components, variable components, communication components, canvas components, third-party interfaces, AI frontend components, backend components, covering all aspects of application development.

#### 8. What is the design philosophy of "Component-Oriented Programming" adopted by VisualLogic, and its advantages?

"Component-Oriented Programming" stands at a higher level than code, better isolating the code and helping non-programmers quickly learn programming.

#### 9. How are VisualLogic components designed?

Each component implements object-oriented encapsulation, including trigger conditions, properties, and functions. Developers can construct application logic by combining component objects through the event panel.

#### 10. Why can VisualLogic generate full-stack code?

VisualLogic converts application logic into AST (Abstract Syntax Tree) through the event panel and data flow panel, ultimately generating both frontend and backend code.

#### 11. What are the expansion capabilities of VisualLogic?

- **Data Connection**: Can connect with other systems for continuous scalability in development.
- **Custom Components**: Supports uploading custom components and maintaining and sharing them in the VisualLogic Market.
- **Code Embedding**: Supports embedding CSS, JS functions, SQL statements, etc.
- **SDK Integration**: Supports uploading Java, Python, Android, Node.js SDK code, and running together with the platform-generated code.
- **Security Features**: Supports JWT login, single sign-on (SSO), WebSocket real-time communication, etc.

#### 12. How does VisualLogic provide a complete set of enterprise-grade solutions from development to deployment and maintenance?

VisualLogic offers a complete set of enterprise-grade solutions, including visual UI configuration, logical development, data object design, preview, publishing, CI/CD environment, logging system, permission management, microservices governance, and multi-tenant management.

#### 13. What is the complexity of VisualLogic, and how does it achieve quantitative management/performance of developers?

The "complexity" of VisualLogic is an indicator to quantify the workload of developers, representing the number of effective operational steps taken by developers. By tracking the development process/steps, VisualLogic can review the development process, aiding in development learning/teaching, process auditing, and quantifying developer performance.

#### 14. What languages can VisualLogic generate code in?

VisualLogic can generate frontend JavaScript code and backend Java, Node.js, and Python code.

#### 15. Does VisualLogic support third-party development packages?

VisualLogic supports embedding program code, writing custom components, and integrating npm packages and SDK packages to extend its functionality on a no-code development basis.

#### 16. How does VisualLogic integrate AI capabilities? How does VisualLogic support AI? In what ways does VisualLogic support AI?

- Encapsulating AI models as VisualLogic components for direct use by developers.
- Converting webpages into VisualLogic front-end objects to reduce front-end layout development time.
- Encapsulating large language models (LLM) as components for user invocation convenience.
- Integrating front-end AI models (MediaPipe, TensorFlow.js) as VisualLogic components.
- Considering integrating the code capabilities provided by LLM into VisualLogic IDE in the future.

#### 17. How are VisualLogic's code snippets encapsulated? How are code snippets used?

VisualLogic's code snippets are implemented by encapsulating common code logic into reusable modules. Developers can add code snippets to applications by dragging and configuring them.

#### 18. How are VisualLogic models encapsulated? What is the working principle of VisualLogic models?

VisualLogic models are implemented by encapsulating elements such as business logic, data structures, and user interfaces into reusable modules. Developers can quickly build applications by configuring models.

### 3. VisualLogic Application Scenarios and Users

#### 19. What are the usage scenarios for VisualLogic?

VisualLogic is suitable for various application scenarios, including enterprise internal management systems, document collaboration, IM, e-commerce platforms, large screen display systems, games, marketing applications, and personal applications.

#### 20. What advantages does VisualLogic have over traditional programming languages?

- **Easier Learning**: Lower learning curve, no need to learn complex syntax and frameworks, just master the use of VisualLogic IDE components and event panel.
- **Higher Development Efficiency**: Using "Component-Oriented Programming" and "Event Panel" technologies eliminates the need for various environment setups, enhancing development efficiency by "mouse programming."
- **Higher Runtime Efficiency**: Frontend generates Vue/React, backend generates Java SpringBoot framework code, ensuring high code runtime efficiency.
- **Higher Maintenance Efficiency**: Using Serverless services and other stateless resources in the overall architecture significantly enhances maintenance efficiency.

#### 21. What is the product positioning and target customers of VisualLogic? What are the main advantages of VisualLogic for different users and customers? What problems does it solve for users and customers?

VisualLogic targets developers and enterprises, providing developers with rapid application development tools and enterprises with efficient development platforms and maintenance environments.

#### 22. Is VisualLogic targeted at enterprises or developers? Who are the target users of VisualLogic?

VisualLogic targets both developers and enterprises, offering developers rapid application development tools and enterprises efficient development platforms and maintenance environments.

#### 23. What applications can VisualLogic primarily develop? What are the applicable application development scenarios for VisualLogic? What development scenarios might not be suitable for VisualLogic? What are some limitations of using VisualLogic?

VisualLogic can theoretically develop any application and is suitable for various application development scenarios. However, currently, it is less suitable for pure algorithmic research due to limited abstraction and encapsulation of pure algorithm components.

#### 24. What products does VisualLogic offer to individual developers?

- **VisualLogic IDE**: Including free and programmer versions, providing full-stack application development functionality.
- **VisualLogic App Store**: Offering resources like component/code snippet markets.
- **VisualLogic Educational Products**: Offering free and paid courses.

#### 25. What products does VisualLogic offer to enterprises? What services does VisualLogic provide to enterprises? How does VisualLogic charge business users?

VisualLogic provides both public and private deployment services, charging based on the number of accounts or offering one-time buyout packages.

#### 26. What products does VisualLogic offer to the education industry? How does it charge? Why does VisualLogic offer many free products to the education and training industry?

VisualLogic provides free educational versions for educational users, with complete functionality, supporting source code export and unlimited users, but cannot be used for commercial purposes.

#### 27. How does VisualLogic charge for its courses? What support services do VisualLogic's paid courses offer?

VisualLogic offers free and paid courses, with paid courses typically including class supervisors, group Q&A, examination services, and VisualLogic competency certificates.

#### 28. Why is it recommended for beginners to start learning programming with VisualLogic? Why is it suggested to learn VisualLogic first instead of Python? What are the advantages of learning VisualLogic compared to Python?

Learning VisualLogic is easier to get started with, allowing quick mastery of full-stack development capabilities. Additionally, VisualLogic provides a crowdsourcing platform where learners can take on outsourcing projects and earn income.

#### 29. Can ChatGPT/GPT-4 or other large language models replace VisualLogic? What are some noticeable shortcomings of ChatGPT/GPT-4 or other large language models? Why are ChatGPT/GPT-4 or other large language models not yet suitable for application development?

Current large language models cannot fully replace VisualLogic, with major shortcomings including:

- Inability to complete the development of large, complex applications.
- Presence of "hallucinations," with potentially erroneous generated code.
- Limited training data for code, making effective code training difficult.
- Interaction using natural language is less efficient.

#### 30. Can VisualLogic integrate with hardware? Can VisualLogic develop products related to hardware/IoT/smart hardware?

VisualLogic can integrate with hardware through browser-provided hardware interfaces or by incorporating frameworks like Flutter, Electron, to develop products related to hardware, IoT, and smart hardware.

#### 31. What are the advantages of using VisualLogic for software development operations and maintenance?

VisualLogic unifies development methods and processes, simplifying operations and maintenance work, reducing reliance on specific technology stacks, and facilitating team collaboration and maintenance.

#### 32. What measures does VisualLogic take in application and code security? How does VisualLogic ensure the secure operation of applications?

- Component mechanism isolates code.
- Encrypted communication.
- Integration with cloud computing platforms.
- Supports code encryption and scanning.

#### 33. How does VisualLogic plan to integrate with AI models, AI technologies/LLM? What are VisualLogic's plans in the field of artificial intelligence? How does VisualLogic integrate with large language models?

VisualLogic plans to make AI models, including large language models (LLM), into components and use VisualLogic's logic panel to edit the logic between components, ultimately generating applications to simplify AI application development.

#### 34. Will applications developed by VisualLogic incur additional charges? Can VisualLogic interfere/monitor/listen to published applications? Will VisualLogic limit the final number of users for applications?

No, once an application is published, the generated code belongs to the user, and VisualLogic will not interfere with the user's application or limit the number of users.

#### 35. What is the runtime environment required for applications/projects developed by VisualLogic?

Applications generated by VisualLogic need to run in an environment supporting the Java Spring Boot framework, with dependencies specified in the build.gradle file in the project root directory.

#### 36. Does VisualLogic support high concurrency? If I have many users, can applications developed by VisualLogic support them? Can VisualLogic directly call/pass through cloud computing capabilities? Why is it said that "VisualLogic only generates code itself, and is decoupled from the resources required for backend runtime"?

VisualLogic applications support high concurrency, with a single core 2G memory supporting 1200 concurrent requests per second. VisualLogic by default uses AWS cloud resources, elastically expanding concurrency capabilities as per user demands. The generated code and runtime resources are decoupled, with concurrency capabilities depending on the deployment environment.

#### 37. Does VisualLogic support collaborative development? Does VisualLogic support version management? What kinds of tool chains does VisualLogic offer?

- VisualLogic supports multi-person collaborative development with Git-like version management functionality.
- It supports version management, allowing automatic and manual saving of multiple development versions, generating a release version post-release. Enterprise users can manage and test all release versions.

#### 38. Does VisualLogic support local private deployment? What methods are available? How is it charged?

VisualLogic supports local private deployment, offering development, testing, and production environments. The pricing model is a one-time buyout based on the number of development accounts.

#### 39. Is the code generated by VisualLogic efficient? What is the quality of VisualLogic's generated code? What kind of code is generated for preview during development? What kind of code is generated after publishing? Are the preview and post-release/export codes the same? What are their characteristics?

- The code generated by VisualLogic is simple, efficient, and has high operating efficiency.
- The preview during development uses WebAssembly technology, requiring no compilation, quickly opening previews.
- After publishing, the project is compiled into Java code, with higher operational efficiency.

#### 40. Can the code generated by VisualLogic be directly used for secondary development in the traditional coding manner?

Yes, the code generated by VisualLogic is highly readable and can be used for secondary development using traditional coding methods.

#### 41. What methods does VisualLogic provide for secondary development?

- Directly through VisualLogic IDE for secondary development.
- Partial development using VisualLogic and partial using traditional coding.
- Pure coding for secondary development of the exported code.

#### 42. Although VisualLogic can be considered a Chinese programming language, how does it differ from other Chinese programming languages? What are the advantages of VisualLogic compared to other Chinese programming languages?

VisualLogic is a new graphical programming language, differing from other Chinese programming languages which only change code syntax and keywords to Chinese. VisualLogic changes the development method, with code being optional.

#### 43. How is the code of VisualLogic generated? What is the process of VisualLogic code generation?

VisualLogic converts the logic from the event panel and data flow panel into AST, ultimately generating frontend and backend code.

#### 44. What is the architecture of the frontend code generated by VisualLogic? What technologies does VisualLogic's frontend code use? How flexible is VisualLogic's frontend? What dependencies are required for VisualLogic's frontend to run?

The frontend code generated by VisualLogic uses the Vue/React framework, supports micro frontend architecture, has high flexibility, and requires a browser environment to run.

#### 45. What is the architecture of the backend code generated by VisualLogic? What technologies does VisualLogic's backend code use? How flexible is VisualLogic's backend? How does VisualLogic integrate with cloud computing and leverage its advantages? What dependencies are required for VisualLogic's backend to run?

The backend code generated by VisualLogic uses the Java Spring Boot framework, has high flexibility, and supports integration with various cloud computing services.

### 4. Specific Applications and Technical Support of VisualLogic Software

#### 47. Can applications generated by VisualLogic be exported for deployment?

Applications developed with VisualLogic can be exported with source code and have a "one-click deployment" feature, maximizing convenience for users deploying code and providing it for free.

#### 48. Enterprise application development and support features of VisualLogic

- VisualLogic provides Git-like version management functionality, supporting collaborative development.
- Supports three runtime environments: development, testing, and production, ensuring stability at different stages.

### 5. VisualLogic's Community and Courses

#### 49. VisualLogic Learning Resources and Community

- **VisualLogic Academy**: Visit [VisualLogic Academy](https://course.ivx.cn/route/home) for basic and advanced courses.
- **Documentation Center**: Visit [VisualLogic Documentation Center](https://doc.ivx.cn/) for comprehensive product and development tips.
- **Demo Center**: Visit [VisualLogic Demo](https://demo.ivx.cn/) for practical demonstrations, facilitating learning.

### 6. Enterprise-Grade Support and Services of VisualLogic

#### 50. Suitable Enterprises for VisualLogic Software

VisualLogic has served well-known enterprises such as Huawei, Alibaba, and Tencent, making it the best choice for efficient development and maintenance for enterprises.

#### 51. Enterprise-Level Custom Services

VisualLogic provides end-to-end enterprise-level custom services from design, development to deployment, supporting multi-tenant environments and multi-language application development.

### 7. VisualLogic Application Flowchart

```mermaid
graph LR
A(VisualLogic Platform Overview) --> B(Products and Features)
B --> C(VisualLogic Software Applications)
C --> D(VisualLogic Community and Courses)
D --> E(Enterprise-Grade Support and Services)
```

Through the content above, users can understand the core advantages, feature designs, and application scenarios of VisualLogic, comprehensively understanding the feasibility and advantages of no-code development using VisualLogic.
 # Deployment Plan for VisualLogic

### VisualLogic Custom SA Independent Deployment Plan

The overall architecture of the SA independent deployment version of the VisualLogic application system includes the following content:

1. **Development Environment**: Includes the VisualLogic editor and all related backend services. In the development environment, application code modifications can be previewed in real-time within the editor, avoiding the wait during the compile and deployment process.
2. **VisualLogic Editor Core IDE**: Provides core functions such as visual editing, front-end and back-end code generation, multi-user collaboration, and developer management.
3. **Application Repository**: After development is completed in the editor, the deployment package can be pushed to the application repository with one click. The application repository manages the application versions within the current organization and can push a specific version to the test environment or production environment.
4. **Test and Production Environments**: Similar in structure, including a k8s container system required to run applications.
5. **Basic Cloud Services**: Including file, database, cache, and message queue (MQ) services. It is recommended to use different database service instances and file services for development, testing, and production.

In addition, this plan includes the following functional modules:

- **Login System**: For user login and management, supporting custom development for third-party logins.
- **Workbench**: A tool for managing applications and viewing system messages.
- **Application Debugging**: Provides complete debugging functions, including API, service debugging, application preview, and logs.
- **Application Export**: Generates front-end and back-end code and resource packages for application deployment in a self-built environment.

### System Architecture

The privately deployed VisualLogic editor system also includes the following modules:

1. **Login System**: An enterprise administrator account will be automatically generated during the initial deployment of VisualLogic. Sub-accounts are allocated according to the limitations set in the deployment plan. The login system supports integration with third-party systems.
2. **Workbench**: For application management and message viewing, supporting the enterprise administrator account to manage all sub-account applications.
3. **Application Export Function**: Supports the export of front-end and back-end code and resources for application deployment in the production environment.

### Hardware Environment Requirements

The VisualLogic editing system requires deployment on a single machine and needs auxiliary security dongles. Specific hardware configurations are as follows:

- **Server Architecture**: X86 architecture is recommended; ARM architecture requires additional payment for security dongles.
- **Recommended Operating System**: OpenEuler 22.03.
- **Recommended Server Performance**:
  - 1-20 users: 8 cores, 16-32G;
  - 20-50 users: 16 cores, 32-64G;
  - 50-200 users: 32 cores, 64-128G.

Supported basic software includes redis, nodejs, mysql, and influx DB. It is recommended to use memory four times that of these services.

### Usage Restrictions

Depending on the chosen plan, usage restrictions include:

1. User number limitations (including all sub-accounts and the main admin account).
2. Application number limitations.
3. Deployment usage period.

### Deployment Practice of VisualLogic Enterprise Application System

Upon deployment of the VisualLogic enterprise application system, the system environment is divided into development, testing, and production, isolated through k8s clusters.

#### Application Running Environment

The system includes three isolated environments for development, testing, and production, each supported by independent k8s clusters. They are structurally identical but serve different purposes:

1. **Development Environment**: Includes the VisualLogic editor and related services, providing dynamic preview functionality for applications.
2. **Testing and Production Environments**: Structurally similar, each with an embedded k8s container system.

#### Application Running Environment Deployment Structure

VisualLogic projects are packaged into docker images and deployed as a k8s service in both testing and production environments. The application structure design relies on cloud services to achieve resource decoupling and application isolation.

#### Detailed CICD Process

The process includes deploying the application repository, pushing application image packages, deploying applications, updating and rolling back applications, logs and monitoring, and canceling deployments, as outlined below:

```mermaid
graph LR
  A[Configure Application Repository] --> B[Push Application Image Package]
  B --> C[Deploy Application]
  C --> D[Application Update and Rollback]
  D --> E[Logs and Monitoring]
  E --> F[Cancel Deployment]
```

Detailed steps are as follows:

1. **Configure Application Repository**: Set the deployment service address and configure the repository administrator.
2. **Push Application Image Package**: After completing application development, push the deployment package to the application repository, involving java source code export, compilation, docker image compilation, etc.
3. **Deploy Application in Application Repository**: Specify deployment environment, environment variables, container specifications, and backend resource configurations.
4. **Application Update and Rollback**: Select different versions for deployment through a one-click deployment interface.
5. **Logs and Monitoring**: The backend log function can integrate with cloud services or local log services for real-time log viewing.
6. **Cancel Deployment**: Cancel the deployed application.

### User Management and Authentication

VisualLogic provides a user center system for user management and authentication. The user center template offers login, management, and permission configuration functions, supporting multi-tenant data isolation.

### Tenant Access Permissions for Data Tables

Multi-tenant shared data tables are implemented via virtual tables, with each subsidiary company only able to access its own data.

### Integration with Existing Projects

VisualLogic supports integration with existing front-end and back-end projects:

1. **Front-end Project Integration**: Embed existing pages through micro front-end containers, supporting event communication and performance optimization.
2. **Back-end Project Integration**: Allows manual modification of service definitions, adding interceptors, and integrating with existing projects through microservices.

### Project Version Management and Multi-user Collaboration

VisualLogic enterprise accounts support multi-user project collaboration, enabling project version management and code pulling/pushing functions.
 # Timeline Panel

### Feature Overview

The timeline panel is the editing panel for creating trajectory animations in VisualLogic. In the timeline panel, we can freely set trajectory animations for one or multiple objects. The timeline itself is meaningless and requires adding trajectories to the objects within it, consolidating the keyframe animations of multiple trajectories.

### Activating the Timeline Property Panel

The timeline panel can be activated in the following five scenarios:

1. Adding a sliding timeline under the canvas. Selecting this sliding timeline will activate the timeline panel.
2. Adding a timeline under the canvas. Selecting this timeline will activate the timeline panel.
3. Adding audio under the canvas. Selecting this audio will activate the timeline panel.
4. Adding video under the canvas. Selecting this video will activate the timeline panel.
5. Directly adding an object under the canvas or adding an object under the aforementioned components within the canvas. After adding a trajectory to this object, selecting the object itself or its trajectory will activate the timeline panel.

### Display Contents of the Timeline Property Panel

* If a sliding timeline/timeline/audio/video is currently selected, the button in the blue box on the panel shows an add marker button.
* If a regular object or its trajectory is currently selected, the buttons in the blue box on the panel are copy keyframe/paste keyframe.

### Basic Editing Area of the Timeline

The basic editing area of the timeline panel includes (from top to bottom): current time, step back, play/pause, step forward, time ruler, playhead, zoom out ruler, zoom in ruler.

#### Current Time, Time Ruler, Playhead

* The left side shows the current time, while the right side displays the time ruler. The yellow pentagon control on the time ruler is the playhead.
* The current time shows the time where the playhead is located, and the time ruler shows the length of the original duration of the currently selected object. When no keyframe is selected, the current time area displays a gray playhead icon.
* Clicking any position on the time ruler or dragging the playhead will instantly update the current time value.

#### Step Back, Play/Pause, Step Forward

* Clicking the left step back button will fine-tune the playhead/current time backward by 0.05 seconds.
* Clicking the right step forward button will fine-tune the playhead/current time forward by 0.05 seconds.
* Clicking the center play button will start playing the timeline animation from the current time.

#### Zoom Out Ruler, Zoom In Ruler

To control the scaling display of the ruler, click the right icon to zoom in and the left icon to zoom out.

### Common Features

The timeline component consolidates the trajectories of multiple objects within it to realize the simultaneous control of keyframe animations of multiple objects. The timeline component is invisible and has no size when created.

### Basic Usage

#### How to Add a Timeline Component

A timeline component can be added under the canvas and relatively positioned containers.

#### Basic Property Settings

1. **Auto Play**: If the timeline animation needs to play after certain interactions, turn off the auto play switch; otherwise, turn it on.
2. **Loop Play**: If the timeline animation should play continuously in a loop, turn on the loop play switch; otherwise, turn it off.
3. **Original Duration**: Set the original duration automatically as needed to control the length of the timeline.
4. **Actual Duration/Play Speed**: To control the speed (increase or decrease) of the edited timeline animation, modify the play speed or directly set the actual duration to control the actual animation length.

#### Specific Usage

Add materials within the timeline and add trajectories for those materials that need to move. Then, within the timeline panel, you can visually edit the corresponding trajectories to set their animations. Specific trajectory operations are detailed in the trajectory component section.

The specific usage process is as follows:

```mermaid
graph TD
    A[Add Timeline Component] --> B[Add Materials]
    B --> C[Add Trajectories to Materials]
    C --> D[Edit Trajectory Animations]
```

### QA Supplement

No QA data content available currently.
 # Database Panel

### Database Panel

The database panel tool provides a clean, intuitive interface for managing and configuring databases. The tool integrates a variety of database management features, making database management and configuration simple and efficient through its clear interface design.

#### Interface Structure

1. **Left Area**: Displays the basic attributes of the currently selected database. This includes the database name, login user information, and project group information, among others. Additionally, users can view and change the database type, such as MySQL or other types.

2. **Central Workspace**: Users can perform specific data editing and structure configuration operations in this area. Users can add, edit, or delete fields, set field names, data types, default values, whether they are primary keys, and so on. The data management function also allows users to directly insert, update, delete, and query database data. The data list supports pagination and sorting, enabling users to view and manage large amounts of data more conveniently.

3. **Right Execution and Operation Area**: Used for executing specific database tasks, such as adding new fields and initiating specific tasks.

#### Field Management

The management of the database begins with the setting of fields. Users can manage fields in VisualLogic through the following operations:

- **Field Type Setting**: The field type needs to be set based on the data that needs to be stored. The default field types supported by the database include text, numeric, image, time, and JSON.

- **Indexes and Search Conditions**: Users can set up indexes for fields to optimize query performance. Users can also set search conditions for fields, such as case sensitivity and enabling full-text search.

Field management not only affects database performance but also plays an important role in database security. For example, by restricting duplicate data insertion and setting fields to be unmodifiable, data accuracy and integrity can be ensured.

#### Data Prefabrication and Import

There are two ways to add prefabricated data supported by the database: manual input and Excel import.

- **Manual Input**: Manually input data by clicking the "+" button in the database table. Be sure to click "Save Changes" to save the data to the database.

- **Excel Import**: When the data volume is large, users can choose to import data using Excel. During import, note that the first row is the field name, and the system will automatically match it with the fields in the database or create new fields.

For bulk data imports, to ensure data consistency and integrity, users can also add a "Data ID" column to overwrite existing data. Additionally, the system sets a maximum data row limit for bulk imports. If this limit is exceeded, you can contact technical support for assistance.

```mermaid
flowchart TD
    A[Start] --> B[Choose Field Addition Method]
    B -->|Manual Input| C[Open Input Interface]
    C --> D[Enter Data]
    D --> E[Save Data]
    B -->|Excel Import| F[Prepare Excel File]
    F --> G[Import Data]
    G --> E
```

#### Database Management

For databases, secure management is key to ensuring data safety and integrity. VisualLogic allows users to manage databases through the following mechanisms:

- **Security Protection**: By setting modification and deletion permissions, effectively protect the database. By verifying submitted users, only allowing them to update or delete the data they submitted.

- **Restrict Submission Records**: Control the new records added to the database by time or frequency limits. This is applicable in scenarios where there is a need to limit data submission frequency, such as vote counting.

The panel provides a development process divided into preview and release versions. Users can test and modify operations in the preview version to ensure these changes won't affect the stability of the production environment. After verification, it can be released to the official version.

In summary, the database panel of VisualLogic, with its excellent design and one-stop management features, allows developers to easily manage databases even without professional programming skills. By simplifying and optimizing the interface, it effectively shortens the development cycle and improves development efficiency. Hope this introduction helps users quickly leverage the powerful features of the database panel.
 # Concepts of Interactive Logic

### Interactive Logic Foundational Concepts

In VisualLogic, the event panel is used to edit the interactive logic between components. Each "interactive logic" consists of two parts: trigger event and response action. For example, when a button component is clicked, an image component will hide. In this case, "button component is clicked" is the trigger event, while "image component hides" is the response action. By configuring interactive logic between components one after another, users can build complex application logic.

In VisualLogic, each component is preset with a series of selectable events and actions (also known as methods). By selecting an event of a component to trigger the action of another component, basic interactive relationships can be configured. Multiple components can set up multiple interactive relationships and even achieve self-triggering, thus presenting various dynamic interactions. It is noteworthy that different types of components have various preset events and actions. For example, button components can trigger events like "click" or "long press," while variable components are specialized for data change events.

### Basic Interactive Logic Configuration

A basic interactive logic can be implemented in the event panel through the following steps:

1. **Add Event to Component**: Select the component you want to add an event to in the object tree and click the event icon in the logic toolbar to add the event. The event panel will automatically open and select that event.
  
2. **Configure Trigger Event**: Select the corresponding trigger event (such as click) from the trigger event dropdown menu in the event panel to complete the configuration of that event.
  
3. **Select Response Component**: Click the mouse icon in the event panel to activate the object tree selection mode, then select the target response component (such as an image component) from the object tree.
  
4. **Configure Response Action**: Select the target action to be executed (such as "toggle display") in the response component. After completing the above settings, the image will toggle display when the button is clicked.

```mermaid
sequenceDiagram
    participant User as User
    participant ObjectTree as Object Tree
    participant EventPanel as Event Panel
    User->>ObjectTree: Select component and add event
    ObjectTree->>EventPanel: Open event panel
    User->>EventPanel: Configure trigger event
    User->>ObjectTree: Select response component
    User->>EventPanel: Configure response action
```

### Multiple Response Actions

In practice, an event is often associated with multiple response actions to achieve complex logic processing. Users can add actions through the action block toolbar or right-click menu in the event panel. When a trigger event links to multiple actions, the actions execute sequentially. If an action is time-consuming (such as a service request or API call), it will wait for the result before continuing to the next action. In VisualLogic, all actions under the event panel execute synchronously. For asynchronous operations, users can use front-end triggers or back-end message queue components.

### Execution Parameters (Input Parameters) of Actions

Most component actions require one or more input parameters when called. For example, the page-turning action requires specifying the page number to turn to, and opening an external link requires providing the URL address. When selecting an action with input parameters in the event panel, the system automatically pops up the parameter input box. Different input parameter formats and structures cater to different action needs, such as detailed filtering conditions for database outputs. By hovering over the small question mark beside the action name, users can refer to the explanation of the action and parameters. Additionally, the return parameters of actions can be referenced in subsequent action blocks after execution.

### Advantages of Logic Representation

VisualLogic adopts an original panel mode and node + connection mode, realized through the event panel and data flow panel for graphical logic representation. This mode not only enhances intuitiveness and flexibility but also improves the maintainability and efficiency of application logic. The event panel is suitable for complex control logic, while the data flow panel helps in handling concurrent data flows. The combination of the two makes application development and generating vast amounts of code lines easier.
 # Conditional execution of actions

## Conditional Execution of Actions

In program writing, conditional judgment is an important component. When an event is triggered, we often do not want to execute a series of actions rigidly but rather choose to execute different actions or pass different parameters to actions based on some current conditions. In VisualLogic, we achieve conditional judgment through condition blocks. There are two commonly used methods to add conditions: one is to select the trigger event or condition block with the mouse and then click the blue "condition" button at the top of the event panel to add a condition, which will add the condition to the end of the event; the second is to hover the mouse over the + sign under the event and click the "condition" button that appears to add a condition block after the event.

### 1. Adding Condition Blocks
In VisualLogic, a condition block automatically brings up Condition 1 when added, which is a conditional judgment branch. Both sides of the relational operator are formula editors. Clicking the dropdown menu on the right side of the relational operator expands the relational operators. The conditions that can be set include whether the conditions are equal to, greater than, contain, belong to, of a certain type, etc. If the formula editors on both sides meet the condition, the branch is executed.

Note that, unlike in version 4.0, only one of the branches in the same condition block will be executed; once any condition is satisfied, subsequent conditions will not be checked. If none of the conditions are met, the remaining branch is executed.

Let's take a grade judgment event as an example. The input box is used to input grades, and based on the grade, it would display "Good", "Pass", or "Fail". First, write the first condition: judge the grade in the input box. If the grade in the input box is greater than or equal to 80, set the result text to "Good"; modify the relational operator to greater than or equal, select the grade input box from the object tree on the left side of the formula editor, and input the number 80 on the right side. In this case, set the action in the condition branch, choose the result text from the object tree as the object, and choose the action as assignment, inputting the string "Good" in the assignment's formula editor.

### 2. Adding Branch Conditions
When setting the logic for displaying "Pass", you need to add a condition branch. Click the "+" icon of the condition block to add a condition branch.

### 3. Adding Multiple Conditions
In Condition 2, write a second condition: if the grade in the input box is greater than or equal to 60 and less than 80, set the result text to "Pass". Since multiple conditions need to be satisfied simultaneously, click the "+" sign on the right side of Condition 2 to add multiple conditions.

### 3.1 Parallel AND Condition
By default, multiple conditions have an "AND" relationship, meaning all conditions under this block need to be satisfied to execute the branch. In the example, the grade in the input box needs to be greater than or equal to 60 and less than 80, which is the application of the "AND" condition.

### 3.2 Parallel OR Condition
Click the down arrow in front of the multiple conditions to switch from an "AND" relationship to an "OR" relationship. Under the "OR" relationship, as long as any one condition is met, the branch executes. For example, if user permissions are greater than 3 or the user is an administrator, the logical branch executes.

### 4. Adding Remaining Conditions
If none of the conditions in the condition block are met, the remaining branch will execute. Click the "+Remaining" icon of the condition block to add a remaining branch and complete the actions in the remaining branch. Then, preview to see the effect.

### 5. Nested Conditions
Sometimes there are nested conditions. For example, in the above case, add a requirement to add a counter. If there is someone with a score of 100, then the counter for full scores increments by 1. Select Condition 1, click the condition tag at the top of the event panel, add a condition within Condition 1, and set the event.

### 6. Nested Multiple Conditions
There can also be nested multiple conditions. For example, in a parallel AND condition, one condition judgment may have an OR relationship (any one condition is satisfied). Click the "+" sign in the box to add a nested condition block, and the "AND"/"OR" before the nested condition block determines whether all conditions or any one condition in the nested block needs to be satisfied. This overall judgment result will then be a condition for the external multiple condition block, thus achieving nested multiple conditions.

```mermaid
graph TD;
    A[Event Triggered] --> B{Conditional Judgment - Condition 1};
    B -- Satisfied --> C[Result Text Set to Good];
    B -- Not Satisfied --> D{Conditional Judgment - Condition 2};
    D -- Satisfied --> E[Result Text Set to Pass];
    D -- Not Satisfied --> F{Remaining Conditions};
    F --> G[Execute Remaining Actions];
```

VisualLogic products provide rich components and flexible panel designs, allowing users without programming experience to efficiently achieve complex application logic through graphical logic arrangements. The no-code development approach, combined with the conditional execution mechanism, brings higher degrees of freedom and operability for application development and interaction design, fully meeting diverse needs.
 # Loop execution of actions

## Loop Execution of Actions

To achieve effective action loops in development, VisualLogic provides a comprehensive looping mechanism that helps us meet complex development needs through various loop types in the event panel. The following will elaborate on the detailed implementation of action loops in VisualLogic.

### 1. Loop Introduction

In actual development, we may encounter situations where we need to repeatedly execute a certain action. Adding actions one by one is not only complicated and tedious but also cannot handle tasks that require determining the number of loops based on real-time conditions. At this point, the loop function provided in VisualLogic's event panel becomes particularly important. According to different needs, VisualLogic mainly provides the following types of loops:

- **Fixed Loop**: Sets a fixed number of times, and the loop automatically terminates after the set number of times.
- **Conditional Loop**: Determines whether to continue looping based on set conditions. The actions inside the loop are executed only when the conditions are met.
- **Array Loop**: Executes the loop while traversing an array, and the loop terminates after traversing the last element.

Adding loops is similar to adding conditions. Simply click the purple "Loop" button at the top of the event panel to add, and place the actions that need to be looped into the loop. By clicking the loop's drop-down menu, different loop types can be selected.

### 2. Fixed Loop

The fixed loop allows us to pre-set the number of loops. Within the loop body, the loop count for each iteration can be referenced through the formula editor, starting from 0, with a maximum value of the loop count minus 1.

#### Example

Assume an initial counter value of 0, going through 6 loops. Each loop lets the counter increment by the current loop count. Since the loop starts from 0, the calculation is 0+0+1+2+3+4+5=15. The final counter value is 15.

### 3. Conditional Loop

Conditional loops allow us to set conditions. The loop will only execute internal actions if the conditions are met. If the conditions are not met, the loop will be interrupted.

#### Precautions

When using conditional loops, make sure to change the variable used for judgment in each action; otherwise, the condition may always be true, resulting in an infinite loop. Ensure that the condition will eventually be false at an appropriate time for the loop to terminate as expected.

#### Example

Consider the condition that an integer variable's initial value is less than 5. In this case, each time the condition is met, the counter increments by 1. However, if the variable's value is not changed within the loop, the system will continue executing this loop, causing the webpage to freeze. To exit the loop correctly, an action must be added after each loop to update the variable's value.

### 4. Array Loop

Array loops are suitable for traversing each element in an array. Within the loop body, the current loop object can be referenced. If the array is a one-dimensional array, the loop object is the value of each element; if it is an object array, the loop object is each object.

#### Example

In an array loop, you can select an array to traverse and operate on each element or object. This provides great flexibility and convenience for data processing.

### 5. Break Loop

In some special cases, we may want to interrupt the loop under specific conditions. By selecting the "Break Loop" option, you can specify break conditions and exit the loop. Using break loops can effectively avoid unnecessary computations, making the logic more efficient. Generally, break loops need to be used together with conditions to reasonably control the execution of the loop.

#### Example

Set a termination condition within the loop body. Once the condition is met, the loop immediately halts and no subsequent actions are executed.

```mermaid
graph TB
  A[Start] --> B[Set Fixed Loop]
  B --> C[Process Conditional Loop]
  C --> D{Condition Met?}
  D -->|No| E[Break Loop]
  E --> F[Exit]
  D -->|Yes| G[Process Array Loop]
  G --> H[Continue Loop]
  H --> B
```

This document provides a comprehensive introduction to the action loop function in VisualLogic. By reasonably utilizing different types of loops and break mechanisms, we can efficiently complete complex logic configurations, making development simpler and more efficient. The graphical programming and rich component features of VisualLogic offer strong development support, allowing both code novices and experienced programmers to easily build high-quality applications.
 # Automatic Log Tracking for Event Blocks

### Overview

The VisualLogic platform provides an efficient automatic log tracking mechanism that spans both the front-end and back-end, enabling developers to conveniently debug and optimize applications. The following content will detail how to enable and utilize the automatic log tracking feature of event blocks in VisualLogic.

#### How to Enable Log Tracking

The automatic log tracking feature of event blocks can be enabled by clicking the Log icon on the right side of the VisualLogic event panel. Whether on the front-end or back-end, log tracking is a powerful tool that helps developers track the execution process of events in real time, thus enhancing debugging and optimization efforts.

- **Front-end:** After enabling log tracking, developers can view relevant information through the console in the browser's inspection mode. This information includes the component name (name) corresponding to the event block that triggered automatic log tracking, the input parameters (input) in the formula editor, and the output (output) returned by the event block.
- **Back-end:** Similarly, the back-end also supports enabling automatic log tracking by clicking the Log icon on the right side of the event panel. Once enabled, additional information can be viewed in the logs for detailed analysis.

#### Front-end Automatic Log Tracking

The front-end automatic log tracking is designed to be user-friendly and informative. Developers can clearly view the detailed process of event execution through the console in the browser's inspection mode.

1. **Enable Log Tracking:** In the VisualLogic event panel, find the target event block and click the Log icon on the right side to enable log tracking.
2. **View Log Information:** Switch to the browser's developer tools (usually activated by the F12 shortcut) and select the Console tab.
3. **Analyze Output Information:** In the console, analyze the output information including:
   - **Component Name (name):** Identify the current processing component.
   - **Input Parameters (input):** View the input data involved in the event block.
   - **Output Result (output):** Understand the returned result after the event block is processed.

Through these real-time log details, developers can quickly pinpoint issues and make precise debugging based on the detailed logs.

#### Back-end Automatic Log Tracking

The back-end log tracking feature is also renowned for its convenience and comprehensiveness. Through the automatic logging feature, developers can monitor and analyze the execution state of back-end logic.

- **How to Enable:** Navigate to the VisualLogic event panel, select the back-end event block to be monitored, and click the Log icon to enable log tracking.
- **View Logs:** Check the additional information in the log panel, such as execution sequences, input and output data, etc., providing a foundation for debugging back-end logic.

#### Using Browser Console for Issue Localization

The browser console is not only a tool for viewing event logs but also provides an effective method for developers to debug and handle issues in real-time. Here are some methods for issue localization in the browser console:

1. **Enable Console:** Press F12 or right-click and select "Inspect" in the browser to start the developer tools, then go to the Console tab.
2. **View JavaScript Errors:** The console automatically displays JavaScript execution errors, which can quickly pinpoint syntax issues or undefined references.
3. **Use Console Functions:** Use `console.log()`, `console.warn()`, `console.error()` to record custom log information, helping to track variable changes and logic flows.
4. **Real-time Editing and Monitoring:** JavaScript statements can be executed directly in the console to view effects in real-time, suitable for temporary checks and hypothesis verification.

Through the console, developers can dynamically and real-time debug and optimize applications, significantly improving development efficiency and application quality.

#### Advantages of Automatic Log Tracking

- **Ease of Use:** Achieves comprehensive log tracking without additional code configuration.
- **Real-time:** Automatically records events as they occur, helping developers understand the application's state instantly.
- **Efficiency:** Quickly locates issues through automatic collection and analysis, allowing for immediate corrective actions.
- **Flexibility:** Supports log tracking on both browser and back-end, suitable for various application scenarios.

Through detailed log recording and analysis functions, the automatic log tracking feature of the VisualLogic platform provides powerful debugging support, making the development and maintenance of complex applications more efficient and reliable.
 # Use Cases of Formula Editor

### Overview of the Formula Editor

The Formula Editor is a powerful tool in VisualLogic, mainly used to input values or expressions in the event panel and property panel. As a unified expression input box, the Formula Editor helps users apply mathematical operations, reference objects and variables, and use preset blocks to input content.

### Main Functions and Usage Scenarios

In VisualLogic, almost any window where users can input content can use the "Formula Editor." For example, binding attribute values, event condition determination, and assignment operations can all be done through the Formula Editor. Specifically:

- **Variable Referencing**: You can reference object attribute values. By selecting any object through the object tree pane, you can reference the needed attributes in the assignment window.
- **Mathematical Operations**: Supports various mathematical operators, such as addition, subtraction, multiplication, division, modulo, etc. Combined with Math objects and functions, it can handle complex mathematical calculations.
- **Built-in Functions**: For example, color, string concatenation, conditional blocks, value mapping, etc., can be achieved through preset blocks.

The Formula Editor supports the editing of dynamic values. Users can choose to call object attribute values and built-in system parameters, among others. Additionally, combining mathematical formulas and operators enables calculations and assignments in various application scenarios.

### Formula Editor Optimization and New Features

With the release of VisualLogic 5.0, the Formula Editor has implemented numerous optimizations and new features, including:

1. **Nested Support**: The Formula Editor now supports nesting, enhancing the flexibility of expression usage.
2. **Synchronous Execution**: After removing callbacks, the front and back end event panels execute synchronously, allowing users to reference all previously executed results.
3. **Introduction of Preset Blocks**: Improves the readability and maintainability of expressions by using preset input blocks like color, string concatenation, conditional blocks, etc., further simplifying formula writing.
4. **Advanced Mathematical Functions**: Supports built-in mathematical functions for diverse data processing, allowing users to quickly achieve target functions by selecting appropriate mathematical formulas from a dropdown menu.

### Practical Applications and Precautions

When using the Formula Editor, pay attention to the rules of operations and referencing methods:

- **During Operations**, use English quotation marks to enclose non-operator content as much as possible to ensure correct calculation results.
- **Variable and Attribute Referencing**, it is recommended to choose from the dropdown menu rather than manually inputting to reduce the error probability.

VisualLogic’s Formula Editor, through its rich functions and flexible usage methods, enhances the user experience, making it more adept at handling complex application logic. As such, it is not only suitable for beginners in no-code development but also very helpful for engineers familiar with programming. This tool enables users to visualize complex business logic efficiently, achieving rapid iteration and flexible adjustments.

### Conclusion

As a key component in VisualLogic's powerful development toolchain, the Formula Editor provides users with flexible and powerful expression processing capabilities. From basic binding references to complex mathematical formula calculations, it offers a seamless development experience across different business scenarios. In future applications, fully utilizing the features of the Formula Editor will greatly improve the efficiency and quality of VisualLogic development projects.
 # Directly assign value

### Function Overview

In the VisualLogic formula editor, direct input of literals is a convenient operational feature that allows users to quickly set text, numerical values, and specific escape sequences (such as `true`, `false`, `null`). This input method not only improves operational efficiency but also simplifies tasks such as field initialization, parameter setting, and preset text display. Users can directly input literals into the text box, supporting strings, integers, Booleans, and null values. For example, inputting "hello" in the target text's value area.

### Direct Input Values (Literals)

Literals are directly filled contents, including text, numerical values, and special values (such as `true`, `false`, `null`). The detailed explanation is as follows:

- **Text Input**: Type a string in the text box, for example, `"hello"`. Note that after directly filling a text (string) in the formula editor, double quotes will be automatically added once the formula editor loses focus to mark it as a string.
- **Numerical Input**: Directly type the desired numerical value, for example, `123`. Once the numerical input is complete and loses focus, it will take effect immediately.
- **Boolean Handling**: Set the state of a Boolean variable by directly inputting `true` or `false`; this value will remain unchanged after losing focus.
- **Null Representation**: You can input `null` to indicate a null state, which is especially important for optional parameters or undefined states.

In such input scenarios, each type of value is displayed in a different color after losing focus to facilitate quick differentiation and recognition.

### Operating Steps

1. **Direct Text Input**:
   - Type the desired text content in the text box.
   - Once the input is confirmed to be accurate, the content will be immediately applied.

2. **Numerical Input**:
   - For numerical fields, directly input the required number.
   - Once the numerical input is complete and loses focus, it will take effect automatically.

3. **Boolean Setting**:
   - Input `true` or `false` to directly set the Boolean state.

4. **Null Input**:
   - Input `null` to mark the null state.

Through this direct input method, VisualLogic enhances user experience and work efficiency, simplifies initialization and parameter configuration tasks in many application scenarios, and highlights the user-friendliness and flexibility of VisualLogic in no-code development processes.
 # Referenced object value

### Reference Object Values

Reference object values are an important feature in VisualLogic, allowing for flexible referencing and manipulation of various objects within the object tree. VisualLogic provides an intuitive and efficient way to build complex application logic through object properties, methods, and variable calculation functions, fully leveraging the advantages of component-based programming.

#### Referencing Object Properties

In the VisualLogic formula editor, users can directly reference the properties of an object within the object tree. For example, setting the value of a text component to the content of an input box. When users enter data into the input box, the text content will update in real-time. Such property binding can also be done in the property panel, ensuring synchronization and consistency of data.

#### Referencing Variable Calculation Functions

Besides object properties, the VisualLogic formula editor also allows referencing variable calculation functions. These functions act like automatic conversion methods, transforming a variable into another data format based on specific rules. For example, converting the length of an array into a string or performing substring and replacement operations on a text variable. Each variable type is equipped with specific calculation functions, enabling users to choose the appropriate function according to their needs.

#### Referencing Object Methods

In VisualLogic, the event panel's formula editor can reference object methods. For instance, obtaining the current character count in an input box or the current scroll distance of a row component. Users can also directly reference the return parameters of an action group or function. By setting the value of a text component to the return result of an action group, users are effectively calling the action group, retrieving its return value, and assigning it to the text. This method is not only flexible but also supports multi-layer nesting, widely used in various complex operations within VisualLogic.

It is important to note that object methods can only be referenced in the event panel and cannot be bound in the property panel. This is to ensure the system can accurately determine when to invoke methods, maintaining the precision and controllability of application logic.

| Reference Type                 | Event Panel | Property Panel |
| ------------------------------ | ----------- | -------------- |
| Object Properties              | ✓           | ✓              |
| Object Methods                 | ✓           | ✗              |
| Variable Calculation Functions | ✓           | ✓              |

Through the above referencing methods, VisualLogic provides a highly flexible and efficient way to help users quickly build complex application logic, significantly enhancing development efficiency and application responsiveness. VisualLogic's graphical programming mode lowers the development threshold, allowing even users without coding knowledge to easily develop applications.
 # Dropdown Menu

When selecting the formula editor, a system default drop-down menu will appear. This drop-down menu includes some preset blocks, allowing users to select various commonly used parameters and tools. These tools can help users build complex logic and functionality in the VisualLogic development environment.

### Functions of the Default Drop-Down Menu

The default drop-down menu provides a series of convenient selection options, mainly including the following:

1. **Return Result Reference**: In the event panel, users can reference the return results of action blocks executed before the current block through the formula editor, such as the return results of APIs or database operations.

2. **Receive Parameters or Event Parameters**: This drop-down menu allows users to reference the received parameters of the current component and obtain these parameters when some events throw additional parameters. For services, there are system default parameters such as accessing user information and request information.

3. **System Parameters**: Users can choose system parameters related to system time, environment variables, current application information, etc., making front-end and back-end development more convenient.

4. **Loop Variables**: In the formula editor within the event panel, when under a loop block, users can select loop variables and, depending on the type of loop, obtain the current loop count or corresponding array elements.

5. **Loop Object**: If the component is nested under a loop container, users can select the current loop object to conveniently reference the current index or array elements.

6. **Preset Input Blocks**: These include some common input tools, such as selecting objects from the object tree, choosing colors, inputting arrays, and objects, etc. Selecting these input tools through the drop-down menu can enhance editing efficiency.

7. **String Concatenation Block**: Used to concatenate different contents into a new string, a fundamental tool for string concatenation operations.

8. **Value Mapping**: Similar to ternary expressions, this maps the value of an expression to another set of values under different conditions, a very useful logical tool.

9. **Conditional Value and Conditional Judgement**: Allows users to set advanced value mapping and conditional judgements, meeting more complex logical needs.

10. **Null Value Judgement**: A simplified ternary expression to handle cases where the value is null.

11. **Mathematical Formulas**: The formula editor provides a series of mathematical functions for quickly implementing mathematical calculation tasks.

### Usage Scenarios

These functions are mainly used to simplify operations and improve efficiency when building application logic. The drop-down menu can greatly expand the capabilities of the formula editor, allowing developers to easily call related parameters and tools without memorizing complex syntax. This design is very friendly not only to developers familiar with code but also to those engaging in no-code programming, enhancing the usability and development speed of the VisualLogic language.

### Specific Process Example

```mermaid
graph TD;
    A[Preset Input Block Select] --> B[Return Result Reference]
    A --> C[Receive Parameters]
    A --> D[System Parameter Select]
    A --> E[Loop Variables]
    B --> F[Get Event Return Parameters]
    C --> G[Set Event Parameter Conditions]
    D --> H[Select Environment Variables]
    E --> I[Select Current Loop Object]
    F --> J[Combine Application Logic]
```

This flexibility allows users to quickly achieve desired functions and effects according to specific needs during the development process. Users can fully utilize the drop-down menu to organize and manage complex application logic intelligently, safely, and efficiently.
 # Mathematical Expression

## Mathematical Expressions

When developing applications using VisualLogic, mathematical operations are a common necessity. VisualLogic provides a convenient way to perform various mathematical calculations through its formula editor. This article will delve into how to use VisualLogic for mathematical operations, highlighting its powerful features and usage scenarios.

### Basic Operators

The formula editor of VisualLogic supports the following basic operators:

- **Addition (+)**: Used to add two values.
- **Subtraction (-)**: Used to subtract one value from another.
- **Multiplication (*)**: Used to multiply two values.
- **Division (/)**: Used to divide one value by another.
- **Modulus (%)**: Used to get the remainder of the division of one value by another. For example, `10 % 3` yields `1`.

These operators can be used for calculations between numeric variables or other numeric elements. For example, suppose you have a general variable, and you want to assign it the value of a numeric variable plus 1, then divide it by the number of rows in an object array. In the formula editor, you can manually input these operators to construct the formula. However, be sure that all input elements in the mathematical formula must be numeric, otherwise, it will not be possible to input.

Mathematical operators are not only applicable to simple numeric calculations but can also be combined with the rich components provided by VisualLogic to build more complex application logic.

### Examples of Mathematical Operations

Here are some scenario examples using basic operators for mathematical calculations:

1. **Total Price Calculation**:
   - Purchase three items priced at 20, 15, and 10 units respectively. Calculate the total price.
   - Total price = 20 + 15 + 10 = 45 units

2. **Discount Calculation**:
   - An item with an original price of 100 units is sold at an 80% discount. Calculate the discounted price.
   - Discounted price = 100 * 0.8 = 80 units

3. **Average Score Calculation**:
   - A student scores 85, 90, and 88 in three subjects respectively. Calculate the average score.
   - Average score = (85 + 90 + 88) / 3 = 87.67

4. **Simple Interest Calculation**:
   - Deposit 1000 units in a bank with an annual interest rate of 5%. Calculate the interest after one year.
   - Interest = 1000 * 0.05 = 50 units

5. **Road Travel Time Estimation (Ignoring Traffic Lights)**:
   - The distance is 150 kilometers, and the average speed of the car is 60 km/h. Estimate the travel time.
   - Time = 150 / 60 = 2.5 hours

6. **Room Carpet Area Calculation**:
   - A room is 5 meters long and 4 meters wide. Calculate the carpet area needed.
   - Area = 5 * 4 = 20 square meters

7. **Profit Calculation**:
   - Bought at 50 units, sold at 70 units. Calculate the profit.
   - Profit = 70 - 50 = 20 units

8. **Sports Event Timing**:
   - A match lasts 1 hour and 30 minutes. Convert this into minutes.
   - Total minutes = 1 * 60 + 30 = 90 minutes

Through these examples, we can see the flexibility and powerful features of VisualLogic in handling mathematical operations. It deeply integrates various basic operators and logical operations into its components, allowing users to achieve complex calculations and logic construction in a graphical interface. This capability makes VisualLogic a powerful tool that can be utilized by both programming novices and experienced developers.
 # Extra menus of any type

### Extra Menu for Any Type

In actual development, we often face complex and diverse data structures. To extract the value of a certain field from a variable, VisualLogic provides a convenient solution, which is achieved through the extra menu of any type in the formula editor. The design of this menu greatly facilitates developers' operations on complex data structures.

#### Usage Scenarios

In development, data structures can take various forms, such as JSON objects. Suppose we need to get the `change` value of the first item in an array. In version V41 of VisualLogic, developers can get this by entering `a.checkInList[0].developData.change` in the formula editor. However, in version V50, the event panel no longer supports free input, so it is necessary to operate by selecting the appropriate variable type.

When using the formula editor, when you select a variable and click the formula editor again, a suitable extra menu for any data type will automatically pop up. This menu will retract automatically if not selected. Menu options include text, one-dimensional arrays, two-dimensional arrays, object arrays, objects, general variables, and boolean variables.

### Functional Advantages

The extra menu for any type in VisualLogic not only provides a variety of data type selections but also offers corresponding methods for each data type. For example:

- **Text**: Rich string operation functions, convenient for text processing.
- **One-dimensional and two-dimensional arrays**: Support operations like traversal, filtering, mapping, etc.
- **Object arrays and objects**: Facilitate operations on internal attributes of objects.

For example, in version V50, to access the value of the `checkInList` object, you can select a certain value of the object in the extra menu for any data type and enter the object key name "checkInList." Through this operation, the data will be processed as an object array. Developers can continue to select methods of the object array to obtain the required value in the deeper data structure, such as ultimately getting the `change` value.

#### New Features in VisualLogic V5.0

Version V5.0 adds three new computational functions to enhance array processing capabilities:

1. **Filter (.filter method)**:
   - Filters array elements according to set conditions and returns a new array that meets the conditions.
   
2. **Map (.map method)**:
   - Generates a new array based on specified processing rules, where elements are processed accordingly.
   
3. **Find (.find method)**:
   - Finds the first array element that meets the conditions and returns its value.

Through these functions and features of VisualLogic, developers can manipulate various types of data structures more flexibly and freely to meet development needs in different scenarios. Regardless of the data structure, the built-in extra menu for any type in VisualLogic and its methods greatly enhance development efficiency and code readability.
 # Utility function

## Utility Functions

### Overview

Utility functions in VisualLogic are used to handle various data processing tasks, covering the following key areas:

- Data cleaning and transformation
- String and text processing
- Date and time operations
- Array and JSON data processing

These features enable developers to perform data operations on the platform with great efficiency and flexibility. The design of VisualLogic utility functions integrates a large number of JavaScript data processing functions to enhance data processing efficiency on the platform.

### Core Features and Usage

**Variable-Specific Methods**

Each data type provides exclusive processing methods. When a user selects a variable, they can choose the required built-in method from a dropdown menu. This design greatly simplifies the process of complex data operations.

**Function Calls**

In the formula editor, users can call utility functions by entering the "." symbol. This mechanism allows users to enter the "." symbol again after the result of one utility function to achieve continuous calling of utility functions, thereby handling more complex logic. For example, by parsing a string into JSON, then obtaining the first element of an array, then getting a value based on the corresponding position of an object, and finally keeping two decimal places—all this logic can be achieved through consecutive use of utility functions.

**Smart Function Search**

Users can directly enter the name of a JavaScript function to call the corresponding utility function. After entering the function name (such as toFixed), the system will automatically match the corresponding function, and users can directly select it from the dropdown menu. This intelligent design greatly improves user convenience when using utility functions.

**Visual Parameter Filling**

Users do not need to pay attention to code syntax; they only need to enter the necessary values in the pop-up parameter filling interface. This design ensures the accuracy and simplicity of function calls.

### Functional Structure

Utility functions are divided into 11 main categories, aimed at covering common data processing needs:

- Type checking and conversion
- Mathematical operations
- Array operations
- String operations
- Boolean operations
- Date and time
- Regular expressions
- JSON processing
- Encoding and decoding
- Generators
- Global functions

This structural design ensures that developers can flexibly use utility functions in different scenarios to handle various complex data tasks. For example, in string data processing, VisualLogic utility functions provide multiple approaches to simplify string processing logic.

### Major Application Scenarios

Utility functions are especially useful for the following application scenarios:

- Data cleaning and transformation: With its powerful function integration ability, data can be cleaned and transformed quickly and efficiently.
- String and text processing: Simplify routine text processing tasks and reduce error rates.
- Date and time operations: Support complex date and time calculations to ensure accurate time processing.
- Array and JSON data processing: Provide flexible and reliable tools to manage and manipulate complex data structures.

### Core Highlights

The design of utility functions is not just for single-function implementation but significantly improves data processing efficiency and flexibility on the platform. By integrating a large number of JavaScript processing functions, utility functions demonstrate VisualLogic’s commitment to simplifying the developer's workflow.

This tool-centric design reflects the advantages of VisualLogic, providing not only the absence of syntactic burden during high logical complexity but also allowing precise data results to be achieved through simple user operations. It is suitable for both developers who wish to quickly produce outcomes and professional developers aiming for code simplification and efficiency improvement.
 # How to do SSO/Auth/Auth2 integration

## VisualLogic's SSO/Auth/Auth2 Integration Mechanism

VisualLogic, as a powerful no-code development platform, provides two flexible integration modes when connecting applications to third-party OAuth2 systems. By adopting a component-oriented programming design philosophy, VisualLogic simplifies the cumbersome integration process into logical connections and settings between components.

### Third-Party OAuth2 Integration Modes

VisualLogic supports the following two modes for integrating third-party OAuth2 systems:

- **Mode 1: Simple Frontend UI Integration**

  In this case, VisualLogic is only used for creating the frontend UI, while the backend services access the third-party APIs. After successfully logging into the third-party OAuth2 system through the VisualLogic frontend system, the API access token can be retrieved in the browser, which can then be used to call the authorized APIs using VisualLogic's frontend API components.

- **Mode 2: Complete Frontend and Backend Integration**
  
  This mode requires developing backend services within the VisualLogic application, relying on the third-party OAuth2 system for authentication. In this scenario, the VisualLogic account system component needs to be introduced. Upon completing the OAuth2 authorization process, this component generates a login token to control access permissions for VisualLogic backend services and frontend controls. The VisualLogic account system component can manage a single application or support multi-application sharing, achieving true Single Sign-On (SSO).

### Frontend OAuth2 Authentication Integration

Regardless of the mode used, the first step in integrating OAuth2 is implementing login authentication on the frontend. The following uses Microsoft's Azure unified authentication interface as an example to outline a standard OAuth2 interface integration process:

1. **Obtain Authorization Code**
2. **Obtain Token**
3. **Obtain User Information**

During the actual implementation, this process must be followed to complete the authentication integration. These simple three steps allow developers to quickly integrate the OAuth2 authentication feature into the VisualLogic application.

### Usage Example: Combination of Self-built Account System and Third-Party Login

In a self-built account system, developers need to manage the account password verification logic themselves. However, when integrating with third-party platforms, such as obtaining openid/unionID via WeChat for login, we can skip creating an account data table in VisualLogic. The specific implementation steps are as follows:

1. Use the third-party API to obtain the authorization code and user information.
2. Use the obtained user information and roles to call VisualLogic's account system to generate a login token.
3. Within the token's validity period, the backend retrieves user information by parsing the token; if the token is invalid, a re-login is required to obtain a new token.

This method not only enhances system flexibility but also strengthens integration capabilities with various authentication systems.

### OAuth2 Interface Integration Example Flowchart

The following is a schematic diagram of the OAuth2 interface integration process drawn using Mermaid Version 9.1.7 syntax:

```mermaid
graph TD;
    A[Initiate OAuth2 Authentication Process] --> B[Obtain Authorization Code];
    B --> C[Use Authorization Code to Obtain Token];
    C --> D[Use Token to Obtain User Information];
    D --> E{User Information Retrieval Complete}
    E --> |Success| F[Generate Login Token];
    F --> G[Access Application];
    E --> |Failure| H[Re-obtain Authorization Code];
```

Through such step-by-step integration, VisualLogic helps developers quickly integrate various authentication methods in diverse application scenarios, highlighting its modular and componentized design advantages. Meanwhile, VisualLogic's powerful scalability also offers higher adaptability for future potential business requirement changes.
 # User Center System Overview

### Overview

VisualLogic system supports the separation of business applications from user management applications, which means that any business application can be configured with a separate user center application. All functions related to user login, authentication, organization, and permission configuration are completed in the user center, while the business application focuses on handling the business logic. This not only enhances the modularity of the application system but also simplifies the development and maintenance of each business application.

A user center can be associated with multiple business applications, and multiple business applications can use the same user center for login verification and unified personnel and permission configurations in the user center. Under a single enterprise account, multiple user centers can be created and managed via the backend resources interface. Created user centers can be bound to business applications, ensuring that all logic related to user login and authentication within a business application is handled in the user center. This move simplifies the logic processing within the application, and the resource access permission rules of the application will be automatically registered in the user center. Administrators can configure the access permissions of services and tables of the application on the permission configuration interface of the user center, improving security.

### Adding a New User Center

The feature of adding a new user center removes traditional user and account system components. VisualLogic user center provides complete user management functions such as user login, organization management, and permission management, which means that any application or set of applications only needs to bind to a single user center to complete all user-related functionalities. The design of the user center not only simplifies the development requirements of user logic within the application but also supports the automatic logging function of both front and backend, making user management more convenient and efficient.

### Multi-User Center

The user center in the VisualLogic system is essentially a regular application, making it impossible for user centers to be associated across tenants. A tenant can define one or more user centers, making the association between applications and user centers flexible and customizable. The services of the user center also support independent customization, and it doesn't necessarily have to be associated with the internal default database, enabling multi-level user management. For instance, above an ordinary user center, a unified group user center can be established to provide authentication services for sub-user centers without managing application permission configurations. This ensures unified management of personnel information within the group, and application permissions can be distributed to sub-agencies based on functionality, achieving more refined permission management.

### User Center Customization

The user center is essentially a VisualLogic application, allowing customization of user login, management, and permission configuration logic. The system comes with a built-in user table for registration and login operations and supports third-party API or microservice login, ensuring flexibility in development and maintenance.

- **User Registration and Login**: The default user table supports field extension or the use of third-party API for login, but it is necessary to maintain the integrity and consistency of the Redis cache.
- **Organizational Structure**: Management personnel and organizational relationships can directly adopt third-party data sources, ensuring business adaptability and flexibility.
- **Permission Configuration**: The system uses an internal admin login system to manage permissions and also supports the integration of external authentication systems to enhance security.
- **Resource Permission Configuration**: In private deployment scenarios, custom development support is provided to help users manage permissions more flexibly in complex business environments.

### User Center Initialization Configuration

When creating a user center, the system will automatically create an app instance by copying from a template application. It is recommended to complete the following configurations to ensure security and functionality completeness:

1. Check and modify the application's environment variables:
  
   - JWTKey: Key for generating login tokens.
   - userCookieName and adminCookieName: Names of the cookies for user login and administrator login.

2. Enable microservices:
   The user center has three default microservices for communication. Ensure these services are enabled to avoid communication failures in private deployment.

3. Deploy and publish:
   The user center needs to run in the same environment as the business applications. For online publication, the user center needs to be released, while for private deployment, the application needs to be exported for deployment.
   
   Flowchart as follows: 

```mermaid
graph TD;
    A[Create User Center] --> B[Check Environment Variables]
    B --> C[Enable Microservices]
    C --> D{Publish Application}
    D --> E[Online Publication]
    D --> F[Private Deployment]
```

### Communication Between User Center and Business Applications

Communication between the user center and business applications is carried out through predefined communication protocols. All user information management in the user center is managed through internal APIs and microservice systems, ensuring secure isolation and efficient communication between different applications. At the same time, the advantage of VisualLogic products lies in their visual development system, providing users with a complete solution from development to deployment and operation. Within the framework provided by VisualLogic, the user center plays a core role, improving the security of user management while simplifying the construction and management processes of business applications.

### ABAC-Based Permission Management

VisualLogic user center employs ABAC (Attribute-Based Access Control) mode for permission management, offering flexibility that surpasses RBAC. By analyzing user attributes, such as organization, position, roles, etc., personalized access rules can be defined. This design further simplifies the permission management process, achieving fine-grained permission control, enhancing security and user management efficiency.

### User Registration and Login Types

The user module supports multiple user registration/login methods: phone number, email, WeChat public account, WeChat Mini Program, etc. Each registration method has its corresponding login credentials. Once user information is confirmed, it cannot be changed, ensuring security and data integrity. During registration, phone or email verification is required to ensure the reliability and security of the registration process.

This diverse login solution greatly improves the user experience of the application, allowing enterprises to flexibly meet the diverse needs of users.

### User Permission Settings

The user permission settings function allows administrators to set access permissions for each backend service, enhancing system security. By defining user roles and types, permissions can be allocated reasonably, and access permissions can be automatically determined based on user types, improving system security and management convenience.

In summary, VisualLogic provides a comprehensive enterprise-level solution from development to deployment, where the user center demonstrates significant advantages in user management and permission configuration, offering a secure and stable solution for complex enterprise applications.
 # User Center and Business Application Communication

### Overview

In the VisualLogic system, communication between the user center and business applications is an essential element. The system employs a flexible and secure approach to manage interactions between user data and applications. Through this mechanism, user information is properly managed while ensuring the security and scalability of applications.

### Default Communication Behavior

Without modifying the user center, the communication between the user center and business applications primarily includes the following components. The user center is responsible for all user information management functions, while business applications communicate by invoking services from the user center. By default, business applications delegate user login, authentication, and other information to the user center. When an application needs to access or process user information, it interacts with the user center through APIs or microservices. This approach simplifies the implementation of business applications while ensuring the secure management of user data.

### Custom Communication Behavior

When the default communication behavior cannot meet specific needs, custom communication behavior is allowed to implement more flexible user management functions within business applications. For example, user login can be completed directly within the business application without redirecting to the user center. To achieve this, the login services and other related services of the user center can be set as public services. This allows these services to be directly invoked by business applications to meet specific business logic requirements. Similarly, other services can be set as public services or microservices for invocation by business applications or other applications.

This customized communication configuration, through APIs and microservices rather than directly cross-application calls to the user center's user tables, ensures data security and ease of maintenance. Any need to access user center data across applications must be realized through APIs or microservices, thus eliminating potential security risks of direct cross-application access.

### Communication Flowchart of User Center and Business Applications

To better understand the communication mechanism between the user center and business applications, the following is a communication flowchart drawn using Mermaid Version 9.1.7 syntax:

```mermaid
graph TD;
    A(User Center) -->|Provides User Services| B(API/Microservices);
    B -->|Invokes Communication Services| C(Business Application);
    C -->|Calls Through API| D(Executes Specific Business Logic);
    B -->|User Data and Security Management| A;
    D -->|Feedback Results| C;
```

### Conclusion

The VisualLogic system ensures good interaction between the user center and business applications through a flexible communication mechanism. This mechanism not only enhances the security and performance of the system but also simplifies the development and maintenance of business applications. Business applications can leverage the services provided by the user center without considering complex user information processing logic, thereby reducing development difficulty. At the same time, custom communication behaviors offer a high degree of flexibility to meet various business needs. For scenarios that require cross-application access to user data or user management operations, a well-designed API and microservice invocation scheme further ensure the overall security and maintainability of the system.
 # User Center Initialization Configuration

### Initial Setup

The User Center is a standard VisualLogic application for managing user login, organization management, permission configuration, and other functions. When a User Center is created, the system automatically copies an application from a template. Although it is usable in preview mode, to enhance security and to adapt to publishing and exporting operations, it is recommended to perform the following configurations after creation:

#### Configuration Steps

1. **View and Edit User Center Application**

   First, enter the User Center application you created to view and edit it, ensuring all settings meet your requirements.
2. **Configure Environment Variables**

   In the User Center application, configure the following environment variables:

   - `JWTKey`: The key used to generate login tokens, it is recommended to change this value for security reasons.
   - `userCookieName`: The cookie name used when a user logs in.
   - `adminCookieName`: The cookie name used when an administrator logs in.

   If these environment variables are not configured, the system will use default values, but it is strongly recommended to change the `JWTKey` to enhance security.
3. **Enable Microservices**

   The User Center includes three default microservices: `registerResource`, `getResources`, and `getAppPlayPath`. For a private deployment export, ensure these three services are enabled because they are used for communication between business applications and the User Center. Do not change the names of these three services, or the business application will not be able to find the corresponding services during private deployment.
4. **Publish/Export and Deploy User Center**

   The User Center needs to work in the same runtime environment as the business application. For online deployment, publish and list the User Center; for private deployment, export and deploy the User Center application.

#### User Center Initial Setup Flowchart

```mermaid
graph TD;
    A[Create User Center] --> B[View and Edit User Center Application]
    B --> C[Configure Environment Variables]
    C --> D[Enable Microservices]
    D --> E[Publish/Export and Deploy User Center]
```

As a core of application management in the VisualLogic system, the User Center supports various login methods and ensures its security and stability through flexible environment variable configurations and microservices. Furthermore, with clear initial setup steps, enterprise users can easily set up and maintain their User Center, improving work efficiency and data security.

### Advantages of VisualLogic Products

The VisualLogic tools offer a comprehensive "no-code" development environment enabling users without programming skills to create powerful applications. Its "component-oriented programming" design philosophy enhances system flexibility and usability, supporting highly customized development to meet diverse user needs. Additionally, VisualLogic supports the generation of code in multiple programming languages, demonstrating advantages in development speed and code quality.

When implementing and maintaining the User Center, the VisualLogic tool simplifies complex user management processes through its graphical programming interface, providing enterprises with a convenient and reliable User Center solution.
 # User login to the user center

### General User Login to User Center Instructions

When using VisualLogic tools to implement general user login to the user center, our primary focus is on how to use the login functionality of the user center to access relevant business applications. The user center provides multiple convenient login methods, enhancing user experience and system security.

#### User Center Login Methods

Directly open the login address of the user center, and the user will enter the general user login page. On this page, the user can choose to log in using either a username and password or an SMS verification code. Additionally, for quick access, the user center will later support login methods through platforms like WeChat, DingTalk, and Enterprise WeChat.

After successful login, the user can access all business applications associated with the user center. The user center also provides a personal page for users to view and edit personal account information.

If a business application has enabled the auto-redirect feature, when a user attempts to access a protected service without being logged in, the system will automatically redirect to the user center for login authentication.

```mermaid
graph LR
  A[Open User Center Login Page] --> B{Choose Login Method}
  B --> C[Username and Password Login]
  B --> D[SMS Verification Code Login]
  B --> E[Third-party Platform Login]
  C --> F[Login Successful]
  D --> F
  E --> F
  F --> G[Access Business Application]
  G --> H{Protected Service?}
  H -->|Yes| I[Redirect to User Center]
  H -->|No| J[Direct Access]
```

### User Login Details

#### User Login Types and Methods

Users can choose the appropriate login method based on their habits and conditions. VisualLogic user components support multiple login methods, including phone number, email, WeChat public account, and WeChat mini-program. Each method has unique login credentials (information sent to the backend for verification during user login), which are recorded in the user table. Each type of user can only correspond to one registration/login method, and it cannot be changed midway.

Specific phone and email logins can be divided into:

1. **Manual User Login**: Users need to enter their username and password to log in (if logging in by phone number, the username is the phone number; if by email, the username is the email address).

2. **Automatic Initialization Login**: After the user's first login, the system will store the user's login token in the client's browser using an HTTP Only cookie. As long as the user does not manually clear the cookie, this login token can be used for automatic login during application initialization, without the need to re-enter the username and password.

```mermaid
sequenceDiagram
  participant U as User
  participant S as System
  U->>S: Submit Username and Password
  alt Auto Login Enabled
    S->>U: Return Login Success and User Information
  else
    S->>U: Return Anonymous Status, Require Manual Login
  end
```

#### Permission Settings

To ensure system security, the user component also provides a permission setting function. The system can define roles for each user, determining the backend services the user can access. For example, a general user may only access basic services, while VIP users and administrators have higher-level access permissions.

### Summary

General user login to the user center is not only simple and easy to use but also offers rich customization and security configuration options. Through VisualLogic's powerful graphical interface and flexible event handling mechanisms, it is easy to implement various business logics, including user login, without involving complex code writing. This not only improves development efficiency but also provides convenience for maintenance and expansion.
 # Admin login to User Center

### Overview

To access the user center for management, administrators need to log in through the user center console's login URL. On the administrator login page, logging in can be accomplished using the administrator credentials pre-configured in the database. In special cases, if this is the first time opening the login page and there is no administrator data in the database, the system will provide an initialization management popup, where the administrator can initialize a super administrator. Once initialization is complete, the administrator can log in to the user center and begin managing users, organizational units, and permissions.

### Administrator Login Process

The process for administrators to log into the user center is as follows:

1. **Open the Login Page**
   Access the administrator login page directly by navigating to the user center console's login URL using a web browser.
2. **Enter Login Credentials**
   On the login page, enter the administrator credentials that have been pre-configured in the database. This administrator account must be set up manually beforehand.
3. **Initial Initialization Prompt**
   If this is your first time visiting the login page and there are no administrator records in the database, the system will automatically display an initialization management popup. Through this window, the administrator can create a super administrator account.
4. **Post-Login Operations**
   After a successful login, the administrator will enter the user center console where they can perform user management, organizational unit setups, and permissions configuration.

Using this centralized management approach, the VisualLogic user center provides a minimalist and efficient user and permissions management experience, offering enterprises and organizations a stable and secure operational environment.

### User Management Features

In the left-side navigation bar of the user center, on the user list page, the administrator can:

- **Browse User Accounts**: View all currently registered and created user accounts displayed in a list format.
- **Create/Import Users**: Manually input user information or bulk import users via an Excel file.
- **Edit User Information**: Modify basic and extended user information and save the updates.
- **Delete User Accounts**: Block or delete user accounts that are no longer needed at any time.
- **View User Access Logs**: Obtain recent access logs of users for auditing and usage analysis.

This design offers comprehensive user management capabilities, ensuring that administrators have complete control over all user activities.

### Organizational Unit Management

User center organizational information is presented in a tree structure, providing an intuitive organizational hierarchy management:

- **Create Departments**: Create new departments, set names, and establish hierarchy by setting parent departments.
- **Import Departments**: Quickly establish organizational structures by bulk importing via an Excel file.
- **Edit and Move Departments**: Support editing department information and reordering departments.

This flexible department management mechanism allows administrators to easily set up and modify organizational structures to adapt to internal company changes.

### Login Flowchart

```mermaid
graph TD;
  A[Open User Center Login URL] --> B{Is there administrator data?};
  B -->|Yes| C[Enter Administrator Credentials];
  B -->|No| D[Initialize Super Administrator];
  D --> E[Complete Administrator Initialization];
  C --> F[Successful Login];
  E --> F;
  F --> G[Enter User Center Console for Management];
```

### FAQ Addendum

If you encounter issues during administrator login, please ensure the browser URL is correct and that administrator account records exist in the database. If you still cannot log in, verify that you are using the correct administrator credentials or contact technical support for further assistance.

VisualLogic's user center features no-code integration and streamlined management processes, significantly enhancing application usability and development efficiency, providing enterprise users with a flexible, secure, and efficient user management solution.
 # Application Permissions Management

## Application Permission Management

In the VisualLogic application permission management, the user center's permission management adopts the ABAC (Attribute-Based Access Control) framework. This method is more flexible compared to traditional RBAC (Role-Based Access Control), as it not only supports using user attributes such as organization, position level, and roles as permission criteria but also allows custom access rules. For example, in RBAC, permissions are granted based on the role, allowing users access to corresponding resources based on their role. In ABAC, however, permissions are determined directly based on the attributes of the user, such as restricting access to users with a position level higher than 5 and belonging to a specific organization during certain periods. This attribute-based approach allows for more fine-grained permission definitions, enhancing resource security management.

### Service and Database Resource Rules Configuration

On the left side of the permission management interface in the user center, the applications bound to the user center are displayed; the right side shows the resource access rules for each application, including both service and database resources.

The **Service Permission Configuration Panel** contains a tree structure of resource names, descriptions, and rules. Each service is auto-generated based on the object tree of its respective application. Service groups are treated as resources and are managed together with services. Each resource can have one or multiple rules set; if not set, it adopts the parent rule, and in case of conflicts between child and parent rules, the child rules prevail.

The **Database Configuration Panel** allows administrators to set rules for tables or fields within an application individually. Clicking the expand arrow will display all configurable fields.

When adding rules, administrators select resources (such as services or service groups) from the window and configure rule conditions by choosing user attributes, roles, departments, etc. Finally, they configure the authorization effect (allow or deny) and the authorization operation.

The options for authorization operations vary depending on the resource type:

- For services: Invocation need not be specified;
- For tables: Options include select all, filter output, add, update, delete;
- For fields: Options include filter output, update.

Rules support JSON configuration, and a visual configuration feature similar to the condition configuration block in the event panel will be available soon.

### Blacklist Mode VS Whitelist Mode

In the permission management page, you can set the control mode for resource access. When an application is newly bound to the user center, the default resource access mode is whitelist, meaning all access is denied by default, and only resources with explicit rules allowing access are open. Alternatively, you can choose blacklist mode, which permits access by default, except for resources specified in the rules (blacklisted resources).

### User Permission Settings

To enhance security and improve backend service protection, user permissions can be configured via the VisualLogic user component. The steps for user permission settings are as follows:

1. **Define User Roles**

   - Backend services can be classified into general, advanced, and management permissions, corresponding to different service access levels.
2. **Define User Types**

   - User types such as regular user, VIP user, or administrator are linked to general, advanced, and advanced plus management permissions respectively, to limit service invocation.
3. **Set User Types**

   - During user registration, set the user type to dynamically adjust permissions.

Once user permission settings are complete, the backend will determine if a user has the right to invoke a particular service based on their user type.

```mermaid
graph LR
    A[Define User Roles] --> B(Define User Types)
    B --> C[Set User Types]
    C --> D[Permission Judgment]
```

### Example: Application of ABAC in Resource Management

By using the ABAC model, users can have their access permissions defined flexibly based on attributes such as position level, work hours, and organizational attributes. For example:

- Users with a position level higher than a certain value and accessing services during specified work hours can use certain services.

With this permission management system, VisualLogic applications not only enhance adaptability to specific business needs but also significantly improve the security and fine-grained control of resource access, allowing application developers to achieve the best balance between flexibility and security.
 # User Center Customization

### User Center Customization

The User Center is a key application module in VisualLogic, providing a series of foundational services such as user login, management, and permissions configuration, while also allowing users to customize related application logic.

#### User Registration and Login

In the User Center, a default user table is provided for user registration and login operations. Developers can add fields to the default user table or choose to bypass this table by completing registration and login via external APIs or microservices. This is especially important for applications that need to integrate with other systems. It is noteworthy that, in the login service, user information must be written into the `userInfo` field, and this field name cannot be changed. Additionally, the step of writing to the Redis cache during login is crucial, as it ensures the temporary storage and retrieval of user information.

#### Organizational Structure

The User Center comes with an organizational structure and personnel relationship management system. The default organizational structure table records organization information, while the organization-personnel relationship table records the association between personnel and organizations. Each user can belong to multiple organizations. If a business needs to obtain related information from a third-party data source, it can choose to call external APIs during login or authorization and write the organization's ID into the `deptId` field of `userInfo` to support organization-based permission management.

#### Application Resource Configuration Permissions

For permission management, the User Center relies on its own admin login system by default. Users can replace the default authorization with custom APIs. However, it should be noted that the UI of the permission configuration interface cannot be customized, and the data storage format must remain consistent, otherwise, some permission configurations may not take effect. In private enterprise deployment environments, where there are customization needs, this logic is modifiable.

#### Enhanced User Management Functions

The new features of the User Center eliminate complex user account system components and integrate comprehensive user login, organization management, and permission management functions. Any single application or a set of applications only needs to bind with one User Center to automatically manage user information without having to repeatedly implement user-related logic within each application.

### Custom Communication Behavior

In certain cases, business applications may need to directly execute the functions of the User Center instead of following the standard path. Custom communication behavior can be implemented so that services like login authentication become publicly accessible external services. This way, other applications can directly call these services. Additionally, access to user-related tables is restricted and must be done through APIs or microservices to ensure the security of user data.

### Custom User Login Types

The User Center component supports multiple registration and login methods, including mobile phone number, email, WeChat Official Account, and WeChat Mini Program. Each method has a unique login credential, for example, logging in with a mobile phone number uses the phone number as the credential. In the user table, the "login type" field indicates the user's registration method, ensuring that each user corresponds to only one registration method.

### User Permission Settings

To enhance the security of backend services, user permission configuration is essential. The configuration of user permissions depends on the set user roles and user types. Specifically, user roles dictate the scope of services that users can access, while user types serve as user identity markers. Through proper configuration, unauthorized access can be effectively prevented, even if the frontend code is compromised, it cannot bypass the permission restrictions.

### Implementing User Center Customization with VisualLogic Tools

The following are the specific steps and processes for implementing User Center customization:

```mermaid
graph TD;
    A[Initialize User Center] --> B[Define User Table Structure]
    B --> C[Configure Organizational Structure]
    C --> D[Implement Permission Management]
    D --> E{Develop Login and Registration}
    E -->|API/Microservices| F[External Data Integration]
    F --> G[Custom Communication Behavior]
    G --> H[Implement Business Application Integration]
```

These customization functions and highly flexible structures enable the VisualLogic User Center to meet the complex business needs of different enterprises, achieving rapid and precise user management.
 # 

### Prerequisites for Code Export

To ensure smooth code export and compilation, please confirm that the following prerequisites have been met:

- **JAVA Environment Configuration**: Ensure that JAVA is installed on your computer, with JAVA 8 or later recommended, and complete the relevant environment configuration.
- **Node Environment Configuration**: Install Node.js and complete the relevant environment configuration.
- **Code Editor**: It is recommended to use Visual Studio Code (hereinafter referred to as vscode) as the code editor.

### Project Export and Source Code Compilation

iVX provides a convenient way to export project source code and compile it. Below are the detailed steps:

#### Frontend and Backend Code Export

1. **Download and Unzip Code Package**  
   - Manually download the code package and unzip it to a local file directory. The download address is as follows: [ivxapp-1.1.zip](https://file3.ih5.cn/demo/ivxapp-1.1.zip)
  
2. **Install Node Dependencies**  
   - Open the unzipped folder with vscode, enter the `IvxApp` directory, and run the command `npm install` to install Node dependencies.

3. **Export Source Code**  
   - In vscode, select "Export Source Code" from the menu bar, and the system will generate a zip package.

4. **Configure Project Structure**  
   - Unzip the contents of the zip file into the `src` subdirectory of the base directory.

5. **Start Development Server**  
   - Run the command `npm run dev` in the `IvxApp` path, then open `http://localhost:8080/` in the browser to view the project results.

```mermaid
flowchart TD
    A[Download and Unzip Code Package] --> B[Open Directory with vscode]
    B --> C[Run npm install]
    C --> D[Select Export Source Code]
    D --> E[Unzip Contents to src Directory]
    E --> F[Run npm run dev]
    F --> G[View Results in Browser]
```

### Backend Source Code Export and Compilation

iVX allows exporting Java backend source code based on the Spring Boot framework, and we can choose to export a complete project or only the current application code based on experimental needs:

- **Complete Project**: Includes all framework-related code and current application code.
- **Current Application Code**: Includes only source files related to the current application.

#### Source Code Export Entry

- In iVX version 5.0, you can select export backend source code under the file menu.

#### Project Structure

After exporting the complete project, iVX organizes directories and files according to the following structure:

- The core part does not need modification, mainly including frontend file packages and packaging instructions.
- Modifiable parts include backend source code and configuration files.
- When compiling the project, you need to access the `src` directory, which includes various generated application resource files.

#### Compilation and Runtime

- Use the Gradle build tool to package the project, ensuring that the minimum version of JDK 8 and Gradle is installed on the system.
- Run `gradle distZip` in the root directory after unzipping the source code to generate a zip file containing all Jar packages and execution scripts.
- Upload the zip file to the server, unzip it to the specified running directory, and start the service to run the application.

### Export Examples and Support

The exported iVX application not only includes Java and Node.js support provided by the code generation tool but also boasts rich frontend framework compatibility. With the advantage of automatic code generation, iVX products can significantly simplify the development process and improve the development experience. On the basis of the exported project, users can further conduct secondary development and adjustments, ensuring the flexibility and scalability of the project while also guaranteeing the quality of the project's code.

#### Support and Optimization

- iVX provides comprehensive tools and editor support, allowing users to fully leverage the advantages of component-based programming and event editors throughout the development cycle.
- For different application scenarios, iVX will continue to support user-customized functions and tools, such as automatic log recording, user management modules, and final application release features.
 # DingTalk Mini Program Release

## Create DingTalk Mini Program

### Create Mini Program

Follow these steps to create a DingTalk mini program:

1. **Register and log in to a DingTalk developer account**
   - Register and log in according to DingTalk's instructions.

2. **Access DingTalk Developer Console**
   - Go to the DingTalk Developer Console (https://open-dev.dingtalk.com).
   - Select "App Development" -> "Enterprise Internal Development" -> "Mini Program".
   - Click "Create Application".

3. **Fill in Basic Application Information**
   - Provide the application name, logo, and description.
   - Choose "Enterprise Internal Self-Development" (DingTalk mini programs supported by VisualLogic are of this type), then click "Next".

4. **Select Development Mode**
   - Choose "Develop Application".
   - Select "Mini Program" as the application type.
   - Enter the legal IP address for invoking DingTalk API, ensuring the IP address is of a private server.

5. **Set PC Homepage Address**
   - Decide whether to display the app on the DingTalk PC Workbench. If needed, fill in the PC homepage address.

### Illustrated Steps
```mermaid
graph TB
  A[Register Account] --> B[Log in to DingTalk Developer Console]
  B --> C[Select App Development]
  C --> D[Create Application]
  D --> E[Fill in Basic Information]
  E --> F[Select Development Mode]
  F --> G[Set PC Homepage Address]
```

## Upload DingTalk Mini Program

### Upload Mini Program

After publishing the DingTalk mini program in VisualLogic, follow these steps to upload the mini program:

1. **Download and Install Developer Tools**
   - Download and install the official DingTalk developer tools.

2. **Open and Upload Mini Program Project Files**
   - Use the developer tools to open the "dingWeb" folder from the compressed file downloaded from VisualLogic.
   - Select the project type as "Enterprise Internal Application".

3. **Associate Project**
   - Associate the project with the newly created mini program.

4. **Modify Project Configuration Files**
   - Update the domain and URL in config.js to the domain and address after private deployment.
   - Change the title and description in pages/index/index.js.

5. **Upload Project**
   - Use the upload button in the developer tools to upload the mini program to the management console, generating a new development version.

### Illustrated Steps
```mermaid
graph TB
  A[Download Developer Tools] --> B[Open Developer Tools]
  B --> C[Upload Mini Program Files]
  C --> D[Associate Project]
  D --> E[Modify Project Configuration]
  E --> F[Upload Project]
```

## Submit for Review

### Submit for Review

1. **Log in to the Mini Program Developer Platform**
   - Go to "Version Management" and click "Submit for Review".

2. **Wait for Approval**
   - Once approved, click "Publish" to officially go live.

### Illustrated Steps
```mermaid
graph TB
  A[Log in to Mini Program Developer Platform] --> B[Enter Version Management]
  B --> C[Click Submit for Review]
  C --> D[Wait for Approval]
  D --> E[Click Publish]
```

## Publish DingTalk Mini Program

### Publish Mini Program

1. **Set as Testing Version**
   - The development version uploaded to the management console can be set as a testing version.
   - The testing version will generate a test QR code, allowing enterprise staff to experience it. Only one testing version can exist at a time for each mini program.

2. **Publish Mini Program**
   - After testing the development or testing version, confirm no issues and publish it live.
   - Once published, an online version is generated for internal enterprise use.
   - There's a delay in publishing updates to the online version; the new version will overwrite the old version.

3. **Confirm Usage Scope**
   - After publishing, the mini program will enter the online version, where you can set the usage scope for internal employees.

4. **View Published Application**
   - Switch to the corresponding enterprise on the DingTalk mobile Workbench to view the published application.

5. **Manage Historical Versions**
   - Each time a new version is published, the current online version is pushed into the historical versions, maintaining several historical versions.

### Illustrated Steps
```mermaid
graph TB
  A[Set as Testing Version] --> B[Publish Mini Program]
  B --> C[Confirm Usage Scope]
  C --> D[View Published Application]
  D --> E[Manage Historical Versions]
```"
 # ByteDance Mini Program
 # # Creating a ByteDance Mini Program

## # 1. Register a ByteDance Account

Developing a ByteDance Mini Program (Douyin Mini Program) using VisualLogic is essentially an embedded webview development process. All content is based on webview, and the mini program itself is just a carrier. First, you need to go to the ByteDance registration center for quick login registration. Choose to register and log in using an email or phone, then successfully log in and enter the developer platform.

## # 2. Create a Mini Program

After logging into the developer platform, select the option to create a mini program. Fill in the mini program's name, ensuring it meets the standards and accurately describes the function and content of the mini program to meet users' expectations regarding its actual function or service. Once these steps are completed, the mini program is successfully created.
 # # Upload ByteDance Mini Program

## # 1. Configure Mini Program Information

Before publishing and exporting the ByteDance mini program from VisualLogic, you need to enter the ByteDance mini program's Appid and AppSecret in the "Configuration" settings. After saving the configuration, publish the case as a ByteDance mini program within VisualLogic. Once the publication is complete, a download box will pop up to download the zip package of the ByteDance mini program project files.

## # 2. Unzip and Import into Developer Tools

Unzip the mini program zip package exported from VisualLogic to obtain the byteDanceWeb folder within it. Then, download and install the ByteDance developer tool, log in with your ByteDance account, and import the byteDanceWeb folder. Once successfully imported, you can test whether the features work properly using the developer tool, and you can also use the "Toutiao app" for real device debugging by scanning the code. Open the Toutiao app, navigate to "My" in the bottom navigation page, and click the "Scan" button to preview the code.

## # 3. Modify Configuration Files

After shelving the case in the VisualLogic workspace, navigate back to the developer tool, open the config.js file, and remove the `?version=N` parameter that comes with the exported mini program. Since the exported zip package is a test version address, you need to shelve it first and then modify the URL in config.js. For example:

```
Before modification:
https://file82c7e019c4b2.v4.h5sys.cn/play/CFWHn7xc?version=1
After modification:
https://file82c7e019c4b2.v4.h5sys.cn/play/CFWHn7xc
```

## # 4. Upload the Mini Program

After completing the above steps, upload the mini program to the "ByteDance Developer Platform." Ensure that all steps have been correctly executed and the application functions can operate normally before publishing it online.

```mermaid
graph TB
  A[Register ByteDance Account] --> B[Create Mini Program]
  B --> C[Configure Mini Program Information]
  C --> D[Unzip Zip Package]
  D --> E[Import into Developer Tools]
  E --> F[Test Functions]
  F --> G[Modify Configuration Files]
  G --> H[Upload Mini Program]
```"
 # Deploy group applications
 # # Group Application Private Deployment

The group application private deployment is fundamentally similar to the standalone application private deployment, but there are some differences in the specific operations. Below, we will detail the operating steps and configuration suggestions for group application private deployment in different versions.

## # Version v4.1 Group Application Private Deployment

- Export applications within the group separately and deploy them on the same server. Use the same VisualLogicBase to run multiple applications within the group, at which point all applications will use the same set of MySQL databases.

## # Version v5.0 Group Application Private Deployment

- Running with VisualLogicBase: The steps are the same as in version v4.1, requiring each application within the group to be exported separately and deployed on the same server.
- Export Java code package: Enter any application within the group, export the Java code package, and all applications within the group will automatically be exported and merged into one ZIP package.

```mermaid
graph TD;
    A[Enter any application within the group] --> B{Select export method};
    B --> |VisualLogicBase operation| C[Export applications within the group separately];
    B --> |Export Java code package| D[Automatically export and merge all group applications];
    C --> E[Deploy to the same server];
    D --> F[Merge all group applications into a ZIP package];
    F --> G[Deploy to the same server];
    E --> H[Use the same VisualLogicBase and MySQL];
    G --> H[Use the same VisualLogicBase and MySQL];
```

### Server Configuration Recommendations

#### Application Private Deployment

Refers to the export of VisualLogic-developed applications for private deployment on your own managed server. Below are the recommended server configurations:

- **Suitable scenarios:** For application private deployment, suitable for individual developers or small to medium-sized enterprises.
- **Recommended Configuration:**
  - Operating System: OpenEuler version 22.03 (recommended for domestic users)
  - Minimum Hardware Configuration: 2-core CPU, 4GB memory, 20GB storage space
  - Suggested Hardware Configuration: 4-core CPU, 8GB memory, 50GB storage space
  - Database: MySQL

### Additional Notes

1. VisualLogic does not charge for private deployment of applications; developers can refer to the documentation for self-completion.
2. As CentOS has ceased maintenance, it is no longer recommended. Domestic users may opt for OpenEuler version 22.03, which better meets localization and Xinchuang requirements.
3. The VisualLogic development platform's customization and deployment services are only available for enterprise users; please consult via WeChat for detailed pricing.
4. It is recommended to use the Chrome browser when accessing tools and web applications developed by VisualLogic.
 # Importing Figma

### Import Design Files Without Plugins

Importing design files from Figma into VisualLogic does not require any plugins. The process is as follows:

1. **File Menu**: In the VisualLogic IDE, click on the "Import Data from Figma" option within the File menu.
2. **Enter Information**: In the popup window, enter the fileId and token.
3. **Obtain fileId and token**:

   - If you are using the Figma desktop application, right-click the design file and select "Copy Link" to obtain the fileId.
   - The location for obtaining the token can be referred to in the figure below (follow the actual acquisition method or update as needed).
4. **Confirm Import**: After entering the fileId and token, click the confirm button. The content of the selected design file will automatically be imported into the current VisualLogic application.

### Using Plugins to Import Design Files in Figma

If you need precise control over the imported content, consider using the "Figma to VisualLogic" plugin. The steps are as follows:

1. **Download Plugin**: Search for the "Figma to VisualLogic" plugin in Figma and click to install it.
2. **Select Content**: Select the specific content in Figma you wish to import into VisualLogic and copy it.
3. **Manual Paste**: Switch to the VisualLogic editor and manually paste the copied content into the object tree or container component of the VisualLogic application.
4. **Plugin Login**: The first time you use the plugin, you need to log into your VisualLogic account. After that, you can authorize directly through the browser without entering the account password again.

### Designing UI with Components

Due to the limitations of the Figma platform, besides images, rectangles, and text, no additional UI components are provided. Therefore, it is recommended to use the component library provided by VisualLogic for design. These component libraries include basic components, extended styles, and sample templates to facilitate design adjustments.

1. **Import Basic Components**: After importing via the "Figma to VisualLogic" plugin, basic components automatically convert to the corresponding VisualLogic component types, facilitating further development.
2. **Manual Adjustments**: Use different component variants or adjust some styles within VisualLogic.
3. **Using the Official Component Library**: The official component library provides a large number of preset components that are highly suitable for use during the design process.

### Notes

- **Copy Time**: The more content selected, the longer the copy time. It is recommended to import into VisualLogic in portions.
- **Layout Settings**: Avoid adding absolute positions in auto layout.
- **Text Effects**: Text will be more compact post-import. It is recommended to set letter spacing to 10%.
- **Handling Repeated Styles**: Change to loop creation in VisualLogic, similar to conditional containers in Figma.
- **Graphic Asset Size**: Ensure images, icons, and other assets do not exceed 2MB. If they exceed, they will be converted into placeholder images.
- **Component Usage**: Use subcomponents (icon is a single diamond) in the design file instead of main components (four diamonds), as main components do not support copying for import into VisualLogic.

### Structural Differences Between Figma and VisualLogic

- By default, Figma designs are in absolute position layouts. For relative positioning or responsive layouts, utilize auto-layout during design.
- Due to structural differences between Figma and VisualLogic applications, some styles cannot be copied with a percentage match in VisualLogic.

### How to Use the Component Library

1. Click the [Open in Figma] button on the right side of the VisualLogic official component library to open the component library.
2. Choose a component style you need, such as a small blue button, and Ctrl+C to copy it.
3. Open your own project, Ctrl+V to paste it. After pasting, you can adjust the style as needed. Once imported into VisualLogic through official components, they automatically convert to the corresponding VisualLogic component types in the editor, facilitating subsequent development.
 # Import PSD

## Import PSD into VisualLogic Tool

### Prerequisites

Before importing a PSD file into the VisualLogic tool, ensure the PSD file meets the following conditions:

1. Remove all empty layers in the PSD file.
2. Use the RGB channel with an 8-bit format in the PSD file.
3. All layers must be rasterized.

### Import Methods

There are two primary methods to import a PSD file into the VisualLogic tool. Regardless of which method is chosen, you must wait for the import process to complete.

#### Method 1: Import via File Menu

1. Locate and click the "File" menu in the upper-left corner of the VisualLogic editor.
2. Select the "Import PSD" option.
3. Upload the PSD file (ensure the file uses the RGB channel with an 8-bit format).
4. In the object tree panel, select a parent object, such as a page or object group.
5. Proceed with importing the PSD file.

#### Method 2: Drag-and-Drop Import

1. Open the blank area of the stage in the VisualLogic editor.
2. Drag the PSD file directly from the PC desktop to the blank area of the stage.
3. Wait for the import process to finish.
 # Code snippet introduction

### Use Scenarios for Code Snippets

Code snippets allow users to engage in zero-code development within VisualLogic, enabling the creation of "wheels" without having to write any code. In VisualLogic 5.0, support for creating small modules is discontinued, unified by using code snippets to implement reusable functional modules. Code snippets primarily offer the following functionalities:

#### 1. Reuse of Application Logic

Code snippets allow for simple encapsulation, uploading, sharing, and reuse of both frontend and backend logic. For instance, a module capable of sending HTTP requests and processing responses can be reused across multiple applications without the need to rewrite the same code from scratch each time.

#### 2. Decoupling of Application Functional Modules

Internally, a code snippet is a closed logical space. The encapsulation provided by code snippets restricts interaction to the root of the snippet, without exposing the specific implementation details. This method of internal-external isolation effectively modularizes complex applications, facilitating maintenance and updates. For example, a complex user authentication module can be encapsulated within a code snippet, allowing external systems to invoke this module without needing to understand its internal authentication process.

#### 3. Real-time Preview of Frontend Features

Frontend code snippets support real-time preview on the VisualLogic stage, including event handling, property binding, and for-loop lists. Developers can conduct fully WYSIWYG real-time development on the stage without needing to preview the entire application. This enhances development efficiency and reduces errors during the development process.

```mermaid
graph TD
    A(Code Snippet Creation) --> B(Encapsulate Frontend and Backend Logic)
    B --> C(Upload and Share)
    C --> D(Reuse in Multiple Applications)
    A --> E(Modularize Applications)
    E --> F(Maintain and Update)
    A --> G(Real-time Preview Features)
    G --> H(Real-time Development on Stage)
```

### Working Principle of Code Snippets

Code snippets work by encapsulating a series of objects within an object tree into a component. Through this encapsulation, an isolated component space is formed, where interactions between internal and external components are entirely separated. For instance, before encapsulation, internal object 1 might be bound to the value of external object 2, and external object 1 might set properties of internal object 2.

After encapsulation, direct interaction between internal and external objects becomes impossible, as all internal objects are contained within a "component shell." Within this shell, external objects can only "see" the snippet's "component shell," and internal objects can also only see this shell outwardly. Therefore, all interactions require mediation through this component shell.

#### Interaction Between Internal and External Objects

For example, if an external object needs to affect an internal object, it must do so by setting properties of the component shell. These properties are further bound internally to affect the behavior of internal objects. Conversely, internal objects need to influence external objects via triggering events on the component shell.

For external objects, the entire code snippet operates as a normal VisualLogic component that doesn't need to consider the internal complexities of the code snippet. Developers can use a code snippet as a regular VisualLogic component without expanding internal logic, which greatly simplifies the development and maintenance workload.

```mermaid
graph TD
    A(Object Tree) --> B(Encapsulate as Component)
    B --> C(Component Space Isolation)
    C --> D(Internal and External Objects Cannot Directly Interact)
    D --> E(Set Properties Through Component Shell)
    D --> F(Trigger Events Through Component Shell)
    E --> G(Affect Internal Objects)
    F --> H(Affect External Objects)
    G --> I(Normal VisualLogic Component)
    H --> I
```"
 # Create snippet

## # Encapsulating Frontend Code Snippets

Within the application, you can single-select or multi-select several objects and right-click to choose "Encapsulate as Code Snippet" to encapsulate the selected objects and their child objects into a single code snippet. For example, if we encapsulate a multi-selection list as a code snippet:

After encapsulation, a new code snippet structure will form in the object tree, which mainly consists of three parts:

1. **Snippet Root**

   This is the root container of the entire code snippet. Once a group of objects is encapsulated as a code snippet, they become an independent VisualLogic component, and the snippet root is the entry point for operating this component. The snippet root includes the component's properties, methods (actions), and events. All interactions between the code snippet and external objects must be conducted through the snippet root. Objects inside the code snippet cannot directly interact with external objects, meaning internal objects cannot directly set or reference external objects and vice versa.

2. **External Interface Container**

   This includes containers for properties, actions, events, and APIs, mainly used to define the external interfaces of the code snippet. We will explain these functions one by one in subsequent chapters.

3. **Original Object Structure**

   These are the objects we multi-selected during encapsulation. In the example, we selected three variable components and one UI component (including a DIV component and its child components). Therefore, the UI structure in the object tree remains unchanged after encapsulation. However, if we select multiple UI components that are not adjacent in the object tree, the encapsulation will forcibly combine these UI components, which may alter the UI layout on the page.

## # Real-time Preview of Code Snippets

If real-time preview is enabled, all events, property bindings, loop creations, service, and API call effects on the current page can be previewed in real-time on the stage. Code snippets can also be previewed in real-time on the editor stage.

- The toggle for real-time preview is located next to the preview button in the top action bar.
- Note that real-time preview may consume additional browser performance and can be manually disabled. (Due to constraints within the editor's environment, real-time preview currently does not support file interface uploads or connection messages.)

## # Handling Interactions During Encapsulation

Since all internal and external interactions can only be realized through the root component after code snippet encapsulation, the existence of such interactions within the object before encapsulation will cause the encapsulation to fail. Some interaction systems will automatically convert to legal interactions during encapsulation, i.e., by transferring through the snippet root's properties, actions, and events. Other interactions, due to changes in the object tree structure, may lead to unexpected results from the system's automatic handling. An error will be prompted in such cases, requiring manual handling before encapsulation.

**Detailed Handling Methods are Listed in the Table Below:**

| Interaction Direction                      | Interaction Method                                            | Handling Method                                                                       | Details                                                                                                                                             |
| ------------------------------------------ | ------------------------------------------------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| External -> Internal                       | External object references internal variable values           | Automatic Handling                                                                     | Internal variables are moved into the property interface container, becoming attributes of the code snippet. Note that the snippet's properties are unidirectional, where external objects can set snippet properties to affect internal objects, but internal objects cannot update properties. |
| External object bound to internal object's property | Error                                                          | Need to unbind first, and later obtain parameters through events.                       |                                                                                                                                                    |
| External object set internal object's property     | Automatic Handling                                             | Internal object properties are converted to snippet root properties. Internal object's corresponding properties bind to snippet root properties. |                                                                                                                                                    |
| External object calls internal action group         | Automatic Handling                                             | Action groups are moved into the action interface container, becoming snippet root actions. |                                                                                                                                                    |
| External object calls an internal object's action | Error                                                          | Needs to convert the call to an action group call before encapsulation.                 |                                                                                                                                                    |
| Internal -> External                       | Internal object bound to external variable                    | Automatic Handling                                                                     | If the external variable is not referenced by other external interactions, it will automatically be included in the snippet. If the variable is externally referenced, it will be moved into the property interface container, becoming a snippet property.                 |
| Internal object bound to external object's property | Error                                                          | It is not recommended to have property bindings between internal and external objects; it is suggested to split them into variable bindings, or include the external object into the snippet through multi-selection. |                                                                                                                                                    |
| Internal object calls external object's method or external action group | Error                                                          | Needs to manually convert the call to a code snippet event or include the external object in the snippet before encapsulation. |                                                                                                                                                    |
| Internal object calls backend services             | Automatic Handling                                             | Service calls are converted to API calls, and APIs are bound to backend services.       |                                                                                                                                                    |

```mermaid
graph TD
    A[Select Object] --> B[Encapsulate as Code Snippet]
    B --> C{Code Snippet Structure}
    C --> D[Snippet Root]
    C --> E[External Interface Container]
    C --> F[Original Object Structure]
    G[Check Interactions] --> I[Handle Interactions]
    I --> L{Interaction Legality}
    L --> |Legal|N[Code Snippet Creation Success]
    L --> |Illegal|M[Prompt Error]
```

### Further Optimization and Expansion

In practice, to enhance the reusability and reliability of code snippets, pay attention to the following points:

1. **Clear Component Boundaries**: Strictly distinguish between the internal structure of the code snippet and the external interface to prevent unintentionally changing the external state.
2. **Proper Dependency Handling**: Ensure all dependencies within the code snippet are appropriately handled to avoid runtime errors after encapsulation.
3. **Performance Awareness**: Although real-time preview provides convenient debugging features, it may consume resources in complex pages. It is recommended to disable it when debugging is complete.
4. **Interface Design**: A well-designed interface not only enhances the reusability of the code snippet but also facilitates subsequent maintenance by reducing coupling.

With the detailed explanation above, it is anticipated that the understanding of creating code snippets will deepen, leading to better utilization of the powerful features provided by VisualLogic in practice.
 # Attributes of the code snippet

### Introduction to Code Snippet Attributes

The attributes of frontend code snippets are used to control the behavior of the snippets through the property panel or to pass data into the snippets externally. For instance, you can set a language attribute to uniformly control the language of the text within the snippet or create a dropdown list attribute to control the options of a menu snippet.

To create attributes for a code snippet, you first need to add variables in the property interface container. All variables added under the property interface container will automatically become attributes of the snippet root.

Internal objects can bind to the variables under these property interface containers, thereby linking to the root attributes of the code snippet. For example, an internal text can bind to public variables such as product and price, allowing you to change the internal text content simply by setting the snippet's attributes.

### Considerations for Using Attributes

1. **Unidirectional Data Flow**: The attribute settings of code snippets are unidirectional, meaning data flows only from the code snippet properties -> property variables -> internal objects, and not in reverse. Thus, modifying the snippet attributes will affect internal objects, but internal objects cannot set the value of attribute variables. If internal data needs to be passed to external components, you should use the event interface container.
   
2. **Default Value Mechanism**: The values defined in property variables are the default values for the corresponding snippet attributes. For example, when the product attribute value is empty, the snippet will use the value defined in the product variable. However, if the product attribute value is not empty, it takes precedence over the value.
   
3. **Unidirectional Binding Feature**: Similar to all other components, code snippet attributes can bind to other variables or objects and can also be modified externally by setting attributes. However, if the attribute value is set to the same value twice, the set action will not execute (this behavior applies to all VisualLogic web components). For example, if there is an internal input box bound to attribute variable 1 and you set variable 1 to "hello" for the first time externally, the content of the input box changes to "hello". But if a user enters new content (e.g., "123") in the input box and then the property is set to "hello" again, the input box content will not revert to "hello" because the variable 1 attribute of the snippet remains "hello". The input box's binding is unidirectional and will not affect this attribute. Therefore, if you need to repeatedly set an attribute to the same value, you can use action groups defined under the action interface container.

### Attribute Setting Process

#### Creating Attributes for Code Snippets

1. Add variables in the property interface container.
2. All variables added in the property interface container become attributes of the snippet root.
3. Internal objects can bind to variables under the property interface container.

#### Changing Internal Text Content

1. Bind internal text to public variables.
2. Set the attributes of the code snippet.
3. By binding public variables, the internal text content of the snippet will change with the attributes.

```mermaid
graph TD
    A[Add Variables to Property Interface Container] --> B[Variables Become Attributes of Snippet Root]
    B --> C[Internal Objects Bind to Property Interface Container Variables]
    C --> D[Set Attributes to Change Internal Text Content]
```

### Example

Suppose there is a code snippet that needs to display product information based on product and price:

- First, add two variables, "product" and "price", under the property interface container;
- Next, bind the internal display text of the code snippet to these two variables;
- Finally, set the "product" and "price" attributes on the property panel, and the display content will update in real-time.

The above explanation shows that by effectively using VisualLogic tool's "code snippet properties" feature, you can easily achieve interaction between the code snippet and external data, utilizing unidirectional data flow and the default value mechanism to make development more efficient and convenient.
 # Actions of the code snippet

### Overview

The actions of the code snippet are defined under the action interface container. All action groups under the action interface container automatically become actions of the code snippet, which can be directly used like public methods of small modules. The action interface container is automatically created when the code snippet is encapsulated, and it does not require manual creation.

### Example

For example, if we add an action group called "Blink" under the action interface container, then this method will appear under the root of the code snippet. 

In the "Blink" action group, we play an internal animation effect. In this way, we use an external button to call the "Blink" action of the code snippet, making the text blink.

### Notes on Using Actions

1. Action groups under the action interface container can only access objects within the code snippet and cannot directly manipulate external objects. To operate on external objects, events need to be defined in the event interface container.
2. Action groups under the action interface container can be called externally as methods of the code snippet or directly used internally as regular action groups.
3. The logic of calling actions from the root of the code snippet externally will not take effect on the stage in real-time preview mode, as it is external logic to the code snippet, unless the external logic is encapsulated within an outer layer of a code snippet.
 # Events of code snippets

### Overview

Code snippet events are used for operating external objects internally or passing internal data to the outside. For example, when an internal button is clicked, an internal API receives a response, or an internal option is selected, we can use the code snippet events to pass the internally triggered events and related data (such as the response data received by the API) to the external environment.

### Adding Custom Events

To pass events and data, you first need to add a custom event component in the code snippet event interface container.

1. We add a custom event component and name it "Value Change." At this point, a "Value Change" event will appear at the root of the code snippet.
2. Then, internally invoke this custom event component, triggering it, which can be understood as sending a message to it.

### Triggering Custom Events

When an internal variable data changes, the "Value Change" custom event is triggered, passing the latest value to it. This allows us to listen to the event of the internal variable data change at the root of the code snippet and obtain the latest value.

For example, when we externally listen to the internal data change, we can assign the latest data to a text field.

### Predefining Event Parameters

The data passed by custom events is predefined through the properties panel of the custom event component.

- Multiple parameters can be defined.
- After definition, parameters can be passed when internally triggering the custom event.
- Finally, when the root event of the code snippet is triggered, these parameters can be accessed externally.

### Considerations for Using Custom Events

1. **Timing of Custom Event Trigger**: It can be entirely determined by internal logic and isn't necessarily just a simple event pass. For example, conditions can be added to internal events, such as triggering the corresponding event only when a button is clicked and the input box value is greater than 0.
2. **Event Logic Defined at the Root of Code Snippet**: Since it’s external logic to the code snippet, it won’t take effect in real-time preview mode unless the code snippet is wrapped by an outer code snippet.

```mermaid
graph TD
  Internal Variable Data Change --> Data Pass to Custom Event Component
  Data Pass to Custom Event Component --> Trigger Custom Event Component
  Trigger Custom Event Component --> Root Listens to Data Change
  Root Listens to Data Change --> Data Handled Externally
```

By following the above steps and methods, we can effectively perform event operations and data transfers in code snippets on the VisualLogic platform, fully leveraging its advantages for efficient no-code or low-code development.
 # Upload and download code snippets

### 1. Upload to the Cloud

After creating code snippets within the application, you can upload them to the cloud. The upload process is straightforward, but be sure to note the following points:

1. **Specify a Category**: When uploading, choose an appropriate category for better management and retrieval of code snippets.
2. **Version Description**: You can add a version description with each upload. The system automatically assigns a version number to each upload, so no additional specification is necessary.
3. **Locate Uploaded Content**: Once uploaded to the cloud, you can find the newly uploaded code snippets under the custom tab in the component panel.

After uploading, the code snippet will be bound to the cloud definition, and the properties panel will also change. Uploaded code snippets will no longer have the "Upload Code Snippet" option, but instead will show "Update Code Snippet" and "Save As Code Snippet" options. Specifically:

- **Update Code Snippet**: You can update the definition of the currently modified code snippet to the cloud.
- **Save As Code Snippet**: Create a new cloud code snippet. Note that after saving, the code snippet in the current application remains bound to the initially uploaded code snippet, meaning each update continues to modify the first uploaded one.

### 2. Update Code Snippets

When a code snippet needs updating, VisualLogic provides a convenient way to update:

1. **Find Update Option**: Uploaded code snippets will show the "Update Code Snippet" option in the properties panel.
2. **Synchronize Changes**: Updating the code snippet synchronizes the modified definition to the cloud.
3. **Save As Option**: If you need to create a new code snippet instead of overwriting the existing one, choose the "Save As Code Snippet".

### 3. Add Cloud Code Snippets and Modify Versions

In the custom section of the component panel, you can view all code snippets under the current account or organization and perform the following actions:

1. **Add Code Snippet**: Click on a code snippet to add it to the current project.

   - For front-end code snippets, a new page is created, and the code snippet is added to it.
   - For back-end code snippets, they are added directly to the back-end root.
2. **Modify Version**: When the version of a cloud code snippet is updated, you can synchronize it using the version management function within the application.

### 4. Synchronize Code Snippets

If the same cloud code snippet has been added multiple times in the current application, you can choose "Update All Snippets" to update multiple code snippets simultaneously. After updating, properties, actions, and events set on the code snippet roots will not be affected, but if the new version removes previously defined actions, any externally called snippet methods will become invalid.

### Flowchart

```mermaid
graph TD
  A[Create Code Snippet] --> B[Choose Upload]
  B --> C[Specify Category and Version Description]
  C --> D[Auto-generate Version Number]
  D --> E[Locate and Bind Code Snippet]
  E --> F[Choose Update or Save As]
  F --> G[Execute Update or Save As Operation]
```

Through these steps, VisualLogic simplifies the upload and download operations of code snippets, enabling developers to manage and apply cloud code snippets more efficiently.
 # Code Snippet User Manual

In the VisualLogic IDE, code snippets refer to code modules that can be reused within applications. The module component panel allows easy selection and use of these code snippets. This article will provide a detailed introduction to adding code snippets, data view structures, data source binding, DB creation and reverse updating, and the three essential elements of code snippets. This will help users fully understand and utilize code snippets to develop applications.

#### 1. Adding Code Snippets

1. In the left module component panel, select the desired code snippet to use, such as a table list code snippet under the table category.
2. Click the add button to add the code snippet to the object tree on the right.

#### 2. Data View Structure

1. Select the frontend code snippet and click the data view structure in the left property panel. If there's a prompt indicating no data source is bound, you can select a backend data table as the data source or click [Create DB and Bind] to bind a data source.
2. If you need to bind your own data table, you must first add a data table in the backend. For example, create a product list data table where the field content can be customized.
3. In the frontend code snippet's data view structure, select the product list data table created in the backend as the data source.
4. You can delete unnecessary fields and add required fields through the management dropdown menu.

#### 3. Creating DB and Binding

1. Clicking [Create DB and Bind] will create a new data table in the backend, with field names identical to those in the current data view structure, and automatically bind it as the data source.

#### 4. Reverse Update DB Structure

1. You can directly add or delete fields in the frontend code snippet’s data view structure.
2. After clicking [Reverse Update DB Structure], the corresponding table structure in the backend will be modified. For example, add an image field (type as resource), click [Reverse Update DB Structure], and confirm the modifications after the content update prompt.
3. Once completed, the new image field will appear in the backend data table.

#### 5. Field Settings

1. **Manage**: Select and deselect fields from the data source.
2. **Delete**: Conveniently deselect a specific field.
3. **Data Source Field Settings**: Choose a field from the bound data source.
4. **Add Field**: Add a new field.
5. **Additional Information**: When creating a code snippet, you can add fields for additional information in the backend database view, such as whether to participate in sorting (sort), whether to display (show), and column width. After entering field values, you can modify them in the frontend data view structure.

#### 6. The Three Essentials of Code Snippets

Code snippets also possess the three essential components of properties, actions, and events:

1. **Properties**: During the creation of a code snippet, variables are added under the property object container. For example, settings for the visibility of view, edit, and delete buttons, the width and height of the list, and the number of entries displayed per page.
2. **Actions**: During the creation process, action groups are added under the actions object container. For example, code snippets include actions to fetch data lists.
3. **Events**: During the creation process, custom events are added under the events object container. For instance, triggers for viewing details and editing, which can capture returned event parameters.

#### Code Snippet Usage Flowchart

```mermaid
graph TD
    A[Select Code Snippet] --> B[Add to Object Tree]
    B --> C[Configure Data View Structure]
    C --> D{Select Data Source}
    D --> |Existing Data Table| E[Bind Data Source]
    D --> |Create New Table| F[Create DB and Bind]
    E --> G[Reverse Update DB Structure]
    F --> G
    G --> H{Field Settings}
    H --> I[Manage Fields]
    H --> J[Modify Fields]
```

The content above provides a comprehensive introduction to the steps and considerations for using code snippets in VisualLogic IDE. By effectively utilizing these features and tools, users can enhance development efficiency and easily achieve logical construction and data management in applications.
 # internal and external interactions

## Encapsulation and Independence of Code Snippets

Once encapsulated, a code snippet forms an independent subspace internally, where external and internal objects cannot directly interact but can engage indirectly via the root of the code snippet as an intermediary.

| Direction            | Interaction Method | Interaction Scenario                                         |
| -------------------- | ------------------ | ------------------------------------------------------------ |
| External -> Internal | Property           | External objects can set properties on the root of the code snippet, further affecting the behavior of internal objects. Like all VisualLogic components, properties can have an initial value set in the editor or be bound to an external variable. |
| External -> Internal | Action             | External objects can invoke actions (methods) on the root of the code snippet to execute specific actions. This action can be defined internally to operate on internal objects, transforming direct manipulation of internal objects into operations on the root of the code snippet. |
| Internal -> External | Event              | Internal objects can trigger a custom internal event, which in turn triggers an event on the root of the code snippet. Events can pass parameters, allowing internal object events and data states to be communicated to the outside. |
| Internal -> External | API                | Internal objects cannot directly call backend services but can do so indirectly via an API component. The frontend API component can be bound to a backend service, enabling internal objects within the code snippet to trigger a backend service call. |

The intermediary logic mentioned in the table above is implemented in code snippets through external interface containers. When a code snippet is created, the following external interface containers are automatically created for defining properties, actions, events, and APIs:

Detailed methods for these definitions will be discussed in subsequent sections.

## Interaction Flow Between External and Internal

### External Object Sets Code Snippet Properties

1. The external object sets properties on the root of the code snippet.
2. The properties are referenced through the root to internal objects.
3. Internal objects adjust their behavior according to property changes.

```mermaid
graph TB
    A[External Object Sets Property] --> B[Property Referenced in Code Snippet Root]
    B --> C[Property Passed to Internal Object]
    C --> D[Internal Object Adjusts Behavior]
```

### External Object Invokes Code Snippet Action

1. The external object invokes an action on the root of the code snippet.
2. The action is passed to internal methods via the root.
3. Internal methods execute and affect internal objects.

```mermaid
graph TB
    A[External Object Invokes Action] --> B[Action Passed to Code Snippet Root]
    B --> C[Code Snippet Root Calls Internal Method]
    C --> D[Internal Method Executes and Affects Internal Object]
```

### Internal Object Triggers Event Passed to External

1. An internal object triggers a custom event.
2. The event is passed to the external via the root of the code snippet.
3. The external object responds to the event.

```mermaid
graph TB
    A[Internal Object Triggers Event] --> B[Event Passed to Code Snippet Root]
    B --> C[Event Passed to External]
    C --> D[External Object Responds to Event]
```

### Internal Object Calls Backend Services Through API

1. The internal object needs to call a backend service.
2. The internal object uses an API component as an intermediary.
3. The API component binds to the backend service and executes the call.

```mermaid
graph TB
    A[Internal Object Needs to Call Backend Service] --> B[Intermediate via API Component]
    B --> C[API Component Binds to Backend Service]
    C --> D[API Component Executes Service Call]
```

Through these interaction methods, effective communication and operations between the internal and external components of a code snippet are achieved, making application logic clearer and more modular, while enhancing code reusability and maintainability.
 # Basic Usage of Modules

### The Role of Micro Modules

Micro modules allow users to create "wheels" with zero code in VisualLogic. Well-crafted micro modules are used in the same way as ordinary VisualLogic components and have attributes, methods, and events. Creating a micro module is similar to building a regular application; a micro module can be understood as an "embedded sub-application" that can be independently updated within other applications.

### Creation and Use of In-App Micro Modules

There are two creation entry points for micro modules under the "Custom" tab in the components panel: you can choose to create an in-app micro module or a cloud micro module. The creation process is similar for both. Let's use the creation of an in-app micro module as an example:

After clicking the "New" button, we further select a front-end micro module to create. Once created, a new "Micro Module" root appears in the object tree, storing all micro module definitions within the current application under this root.

The micro module root can have multiple micro module containers, each holding a micro module definition. Each micro module can be understood as an independent "sub-application," with no interconnections. Switching between different micro modules allows for editing the current micro module's logic. Each micro module container has three default containers: Public Data, Public Methods, and Custom Events, which are used to define the micro module's attributes, methods, and events. We will elaborate on these in the following sections. Here, we first add several basic components to the micro module:

As shown in the image above, we added a row, a text, and an input box to the micro module and renamed it to "Input Micro Module." After these operations, returning to the main project front-end root, you can find our newly created micro module under the "In-App Micro Module" section in the components panel. Clicking it will add it to the application:

After being added to the application, the input micro module appears as a regular component and can't be expanded like it is under the micro module root. Just like other components, each micro module can be added multiple times in an application:

Next, we return to the micro module root to make a few modifications to the micro module definition:

In the figure above, we added a red asterisk next to the name. At this point, if we switch back to the application, we will find that all three previously added micro modules have the asterisk added:

This is because we modified the micro module definition under the micro module root, which means all instances of that definition used in the current application are also updated. Here we emphasize two important concepts in the micro modules system: micro module definition and micro module instance.

Micro module definition refers to the "creation state" of a micro module, representing the internal content added under the micro module root. There can only be one version of a micro module definition within an application.

Micro module instance refers to the "usage state" of a micro module, meaning the micro module components added within an application (either front-end or back-end root). An application can have multiple micro module instances corresponding to a single micro module definition. When the definition is modified, all instances are synchronously updated.

### Uploading and Downloading Cloud Micro Modules

After creating a micro module locally, it can be uploaded to the cloud for sharing among different applications and sub-accounts within the same enterprise.

Under the "Cloud Micro Modules" directory in the "Custom" tab, you can see all micro modules uploaded by yourself or other sub-accounts within the enterprise:

These micro modules can be added to applications just like ordinary components. Note that each time you add a new cloud micro module, the VisualLogic editor performs the following steps:

1. Download the micro module definition from the cloud.
2. Add the micro module definition to the application.
3. Add an instance of the micro module.

Therefore, each time a new cloud micro module is added, besides adding an instance to the application, the micro module's definition is also added under the micro module root, and its icon appears under the "In-App Micro Module" section in the components panel. In other words, a cloud micro module cannot be directly used in an application but is first downloaded as an in-app micro module definition before being added to the application. Whether created in-app or as a cloud micro module, the flow of use is the same, as shown in the diagram below:

```mermaid
graph LR
subgraph New
    A[In-App Micro Module] -->|Create| B[In-App Micro Module Definition]
    C[Cloud Micro Module Library] -->|Download| B
end
B -->|Add Micro Module Instance| D[VisualLogic Application]
```

Once downloaded, a cloud micro module becomes an in-app micro module. Therefore, modifying the definition of a downloaded cloud micro module does not impact the cloud micro module definition. To update the cloud micro module definition, you need to manually click the "Update Cloud Micro Module" button. Note that for enterprise users, only cloud micro modules created by the current sub-account will show the "Update Cloud Micro Module" button after downloading to the local environment.
 # Attributes, Methods&Events of Modules

### Submodule Properties

Submodule properties are used to configure the internal data and behavior of a submodule. Below is an example of a text input submodule, demonstrating how to add and use properties:

1. **Title:** The placeholder text on the left side of the input box, which users can freely set.
2. **Content:** The actual content of the input box, allowing users to preset and read the input content in real-time.
3. **Required:** Whether the input box can be empty. A red asterisk will be displayed if required, and a prompt will appear when the content is empty.

#### Setting Properties

We add three variables in the public data container, each of which automatically becomes a property in the submodule instance. The property name is equal to the variable name. These variables are called "public variables."

1. **Add Variables:**
   - Add "Title," "Content," and "Required" variables in the public data container.
2. **Bind Properties:** Bind variables to the properties of the submodule component. For example, bind the "Title" variable to the property of the text on the left side of the input box.
3. **Set Initial Values:** You can set initial values in the variables to ensure that certain properties have default values when the submodule instance is added (e.g., initial value "Email").

#### Handling Two-Way Properties

For components like input boxes that involve user input, two-way property binding is usually involved:

1. **Bind Public Variables:** For example, bind the "Content" public variable to the content of the input box.
2. **Update Property Value:** After the user completes the input action, update the input value to the "Content" public variable to ensure synchronization between the input and the submodule property.

For example, bind the input box content with the "Content" public variable and update the public variable value when the input box loses focus.

Below is a flowchart for handling two-way properties:

```mermaid
flowchart TD
    A[Add Public Variable] --> B[Bind Property Content]
    B --> C[Input Box Blur Event]
    C --> D[Update Public Variable Value]
    D --> E[Validate Two-Way Property]
```

#### Adding Remarks

To enhance user understanding, you can add remarks to public variables, which will be displayed in the submodule instance's property panel.

### Submodule Methods (Actions)

By adding an action group under the "Public Method" object container, you can implement submodule methods. Below is an example of a focus method for an input box:

1. **Add Action Group:** Add an action group named "Input Box Focus" under public methods.
2. **Implement Method:** Call the focus method of the input box within the action group.
3. **Add Parameters:** Add an input parameter "Select" to specify the state when the input box is focused.
4. **Set Return Parameters:** If the method calls an API, the API return result can be set as the public method's return result.

For example, bind the "Input Box Focus" method, call the focus method within the action group, and determine the call content based on the input parameter value.

Below is a flowchart for implementing methods with parameters:

```mermaid
flowchart TD
    A[Add Action Group] --> B[Call Focus Method]
    B --> C[Add Input Parameter]
    C --> D[Call According to Input]
    D --> E[Set Return Parameter]
    E --> F[Add Remarks]
```

### Submodule Events

Similar to properties and methods, submodule events are implemented by adding event components under the "Custom Event" container. For example, to implement an "Input Change" event for a text input:

1. **Add Custom Event Component:** Add an event component in the "Custom Event" container.
2. **Implement Event Logic:** For instance, when the input box "Input Change" event triggers, let the custom event trigger, transmitting the input change behavior.
3. **Define Parameters:** You can define parameters for custom events, such as "Input Content" parameter.
4. **Pass Parameters:** Pass the current input content to the parameter when the custom event is triggered.

For example, bind a custom event to the input box's "Input Change" event and pass the input content to the "Input Content" parameter.

Below is a flowchart for setting and triggering custom events:

```mermaid
flowchart TD
    A[Add Custom Event] --> B[Bind Input Change Event]
    B --> C[Define Event Parameter]
    C --> D[Pass Current Input Content]
    D --> E[Trigger Custom Event in Instance]
```

With the structured approach above, the properties, methods, and events of submodules can be configured and utilized reasonably, enabling their application in VisualLogic graphical programming.
 # How to Ensure the Secure Operation of Deployed Applications

### Overview

In managing deployed applications, ensuring their secure operation is a critical task. Below are some common risks and handling methods that need attention to ensure the stability and security of applications.

### Common Risks and Errors

**Risk 1: Modifying Database Structure**

Modifying the database structure is a common and complex risk. For example, deleting or modifying fields of a deployed application may lead to the loss or error of associated data. Since all test and release versions share a single database, any modification to the database structure will directly affect the live application. Generally, adding new fields is considered safe as it does not affect existing logic. However, deleting or adjusting field order may cause unforeseen issues. Therefore, modifying the database structure must be handled cautiously to avoid unnecessary changes.

**Risk 2: Modifying Database or Application Configurations**

All test and live release versions share database and other backend configurations, such as custom domain names. This means any modifications to these configurations will affect the online version. For example, changing domain names may result in application inaccessibility. Therefore, it is not recommended to casually modify these configurations for deployed applications.

**Risk 3: Critical Database Changes by Other Users**

For convenience in data sharing, important databases are often set as account databases. This poses the risk of being modified by those who may not understand their importance. For instance, the development team or clients might make changes without realizing their significance. It is recommended to use private databases or share data via public services to reduce risks.

**Risk 4: Clearing or Modifying Release Version Data**

Once an application generates formal production data, avoid human modification or clearing. Multiple data tables often have associations, and inappropriate modifications can cause severe data anomalies. If changes are necessary, it is recommended to back up the data beforehand. Additionally, managing account permissions can restrict developers' ability to modify production environment data, ensuring stable application operation.

### Methods to Recover from Mistakes

The VisualLogic platform provides some methods to recover from errors caused by inappropriate operations. Within a certain timeframe, it is possible to recover data mistakenly deleted in the release version, although this may involve some costs. For enterprise accounts, the version management list allows administrators to quickly roll back to a previous version in case of errors in the new version. This rollback is limited to the frontend and backend code logic and does not include backend configurations; to rollback backend configurations, a manual restoration to the previous setup is required.

### Steps to Ensure Secure Operation

Here is a recommended procedure flow to ensure the secure operation of online applications:

1. **Review Structure Changes**: Carefully review any changes to database and application configurations before release.
2. **Set Data Permissions**: Use permission management to restrict personnel who can directly modify data.
3. **Backup and Testing**: Always back up data and conduct comprehensive testing in a separate testing environment before making any changes to the production environment.
4. **Monitoring and Logging**: Continuously monitor the production environment and maintain detailed logs for tracking purposes.
5. **Version Management**: Utilize VisualLogic's version control to retain historical versions of the application for quick rollback to a secure version in case issues arise.

```mermaid
graph TD
    A[Enter Application Release Process] --> B(Review Structure Changes)
    B --> C(Set Data Permissions)
    C --> D(Backup and Testing)
    D --> E(Monitoring and Logging)
    E --> F[End Application Release Process]
    D --> G(Version Management)
    G --> F
```

By implementing the above measures, you can effectively ensure the secure operation of deployed applications and reduce issues caused by human error.
 # Methods for Web Testing

## Overview

Testing web applications in a browser is quite convenient, as pressing the F12 key typically opens the developer tools. These tools provide detailed inspection and editing functionalities for web code, including element inspection, style editing, and network monitoring. Most developer tools can also be used to simulate different devices or screen sizes, enabling developers to preview how pages appear on different devices during development or responsive design testing. Below are some commonly used methods for web preview testing.

## Simulate Mobile Devices

1. After opening the developer tools in the browser by pressing F12, you can simulate different device screen sizes and resolutions by clicking the device icon (Toggle device toolbar) in the red box. For example, when previewing a mobile application, clicking this device icon will switch the preview interface to a phone size.
2. The Toggle device toolbar provides a variety of popular device sizes, automatically simulating the display effect of the selected device.

## Console Log Printing

Developers can use the Console to print out necessary information to check data, progressively locate, and troubleshoot issues. There are two methods to set up logging in the editor:

1. Print through console log-related actions in the event panel, such as the system's console print action and the current value action for variables.
2. Click the Log icon on the left side of the event panel. Once enabled, this will automatically print the component name (name) of the current block, the input parameters in the formula editor (input), and the returned results (output) to the browser console.
3. After setting up printing using the methods above, you can view the printed content in the Console by pressing the F12 key during application preview.

## Network Request Inspection

The Network feature of the browser F12 developer tools provides a comprehensive view of the network requests made during web page loading. This feature can be used for the following tests and analyses:

1. **View the return values of interfaces**: Check the data returned from the server, which is very useful for testing API interfaces.
2. **Check the request headers and response headers of interfaces**: View the HTTP header information of requests and responses, which is helpful for analyzing the interaction between server responses and client requests.
3. **Analyze the loading speed of resources**: Evaluate resource loading speeds by analyzing the response times of network requests, which is crucial for optimizing web page performance.
4. **Evaluate resource size and caching status**: View the size of each resource and whether it is cached, which is useful for assessing bandwidth usage and resource management strategies.
5. **Analyze CDN, waiting times, etc.**: By analyzing each stage of requests in detail, key factors that affect page loading speeds, such as CDN effectiveness and waiting times, can be identified.

   - Click the Name of a resource to view its detailed information. The Header section displays the HTTP header information of that resource.
   - The Preview section shows a preview based on the selected resource type (JSON, image, text), for example, displaying the return result of a backend service "get book list".
   - For more information on Cookies, Timing, and other tabs, please search and learn independently online.

```mermaid
flowchart TD
  A[Open Browser Developer Tools] --> B[Simulate Mobile Devices]
  B --> C[Toggle Device Toolbar]
  A --> D[Console Log Printing]
  D --> E[Set in Event Panel]
  D --> F[Enable Log Icon]
  A --> G[Network Request Inspection]
  G --> H[View Interface Return Values]
  G --> I[Check Request and Response Headers]
  G --> J[Analyze Resource Loading Speed]
  G --> K[Evaluate Resource Size and Caching Status]
  G --> L[Analyze CDN and Waiting Times]
```

Above are the commonly used methods for web preview testing. These methods not only help developers rapidly locate issues during development but also provide strong support for page performance optimization.
 # WeChat Pay Configuration

### WeChat Pay Configuration

#### 1. Overview of WeChat Pay

Before starting with WeChat Pay, you need to apply for the appropriate WeChat app depending on the application type (Official Account, Mini Program, or Native APP) and obtain a WeChat Merchant ID to complete the application configuration. The WeChat Merchant Platform offers various payment features, including APP Payment, H5 Payment, Native Payment, and Mini Program Payment. Developers can refer to WeChat's official documentation for more detailed instructions and guidance.

#### 2. Main Steps to Connect WeChat Pay and Configure Merchant ID

1. **Register a WeChat Merchant Platform Account:**
   If you don't have a WeChat Merchant Platform account, you need to register on the WeChat Merchant Platform.

2. **Obtain Merchant ID:**
   Upon registration and completing Merchant Platform verification, you will receive a Merchant ID, which is the basic identification for WeChat Pay.

3. **Configure API Key:**
   Set up the API key in the Merchant Platform under [Account Center] -> [API Security]. Remember the 32-digit key you set as it cannot be viewed and only can be reset.

4. **Download and Configure Merchant Certificate:**
   Download the API certificate from the Merchant Platform and configure the merchant certificate serial number and private key in your system for signature validation of payment requests.

5. **Associate with APPID:**
   Associate the Merchant ID with the current WeChat Official Account, Mini Program, or APP's APPID. This process can be completed in the development settings of the respective platform.

6. **Set Payment Authorization Directory and Authorized Domain:**
   - **Payment Authorization Directory:** Set the page URL associated with payment on the WeChat Merchant Platform. The directory must match the configuration.
   - **Authorized Domain:** To obtain the user openid, set the domain for obtaining openid in the WeChat Official Platform and ensure it is a valid domain.

7. **Enable Payment in the Product Center:**
   Enable required payment features like JSAPI payments on the WeChat Merchant Platform to use them in the Official Account.

#### 3. Method to Fill in WeChat Merchant ID in the Application Configuration

In the VisualLogic editor, click "Configure" and fill in the following information in the popup window:
- **Merchant ID:** Located in "Account Center > Merchant Information > Account Information."
- **Merchant Name:** Can be freely filled (appears on the cover name of red envelopes).
- **Merchant API KEY:** Reset password in "Account Center > API Security > API Key" to obtain.
- **Merchant PemKey/PemCert:** Download the certificate in "API Security > API Certificate," then find relevant file content after extraction.
- **Merchant Secret Key:** Should be consistent with the Merchant API KEY.

#### 4. Set the Payment Authorization Directory and Domain

##### Setting the Payment Authorization Directory

The payment authorization directory is the root directory of the page where the merchant initiates the payment request. For example, for "https://www.weixin.com/pay.php," the authorization directory is "https://www.weixin.com/". When using the payment feature, the actual request page directory must match the directory set on the Merchant Platform, otherwise, it will show an error "The current page's URL is not registered."

##### Setting the Authorized Domain

When developing JSAPI payments, you need to set the authorized domain on the Official Platform to obtain openid, ensuring the domain set is valid for obtaining openid.

#### 5. Overview of the Payment Process

Once the account information is ready, you can create a WeChat payment logic. The typical payment process includes the following steps:

1. **Generate Payment Order at Backend:**
   - Pass the client's IP address and generate the payment order number through the service, then return payment information to the frontend.

2. **Invoke the Payment Interface on Frontend:**
   - The frontend obtains payment information and calls the appropriate method through the "WeChat Official Account" or "WeChat APP Interface" to evoke the payment interface.

3. **User Completes/Abandons Payment:**
   - After the user completes the payment, WeChat sends the payment result to a specified server address. The backend can actively query the order status.

4. **Backend Receives Payment Notification:**
   - The pre-set callback address receives the payment completion notification.

5. **Active Order Status Query:**
   - Query the payment order status based on the merchant order number.

6. **Refund:**
   - If necessary, submit the order number and amount for the refund request.

In summary, this detailed guide to integrating and configuring WeChat Pay helps developers effectively complete the integration of WeChat Pay functions and smoothly manage payment operations through precise step-by-step instructions.
 # Export react/vue component

### Overview

The VisualLogic frontend module can be exported as components for the Vue/React frameworks, allowing developers to directly integrate them into Vue or React code development projects. This section will provide a detailed guide on how to export modules in VisualLogic as Vue/React components and the specific steps to integrate the exported files into Vue/React projects.

### Module Creation Requirements

Before exporting, ensure that the module adheres to naming conventions. In modules intended for export, names for events, methods, and data must consist of a combination of uppercase and lowercase letters, underscores, and numbers, with the first character being a letter. This convention ensures compatibility of the components across different frameworks and development environments.

### Export Operation

1. Open the VisualLogic IDE and navigate to the object tree panel.
2. Select the root node component of the module you wish to export.
3. Check the left properties panel to confirm the appearance of the "Export Vue Component" and "Export React Component" buttons.
4. Click the "Export Vue Component" button to export the component code for the Vue framework; click the "Export React Component" button to export the component code for the React framework.
5. The exported component code will be provided as a sample ZIP package, which developers can download for reference.

Here is a visual representation of the entire process:

```mermaid
graph LR
A[VisualLogic IDE] --> B[Select Module Root Node]
B --> C[Check Properties Panel]
C --> D{Choose Export Type}
D -->|Vue| E[Export Vue Component]
D -->|React| F[Export React Component]
E --> G[Generate Sample ZIP Package]
F --> G[Generate Sample ZIP Package]
```

### Integrating the Exported Component into a Project

The exported components can be directly unzipped and imported into Vue or React development projects, typically by specifying the path to the component within the project. In specific projects, further code adaptation may be required based on storage structure and import locations.

Through this method, VisualLogic not only enhances component reusability and development efficiency but also reduces the overall development difficulty of the project, allowing developers to focus more on functionality implementation and business logic.
 # Importing Vue Modules

### Environment Setup

On the Windows platform, we will demonstrate how to build a Vue project using the Vite tool. For basic usage of Vite, you can refer to [here](https://vitejs.dev/guide/).

#### Creating a Vue Project

You can create a new Vue project based on your package manager using the following commands:

- **npm 6.x**:
  ```bash
  npm create vite@latest my-vue-app --template vue
  ```

- **npm 7+**:
  ```bash
  npm create vite@latest my-vue-app -- --template vue
  ```

- **yarn**:
  ```bash
  yarn create vite my-vue-app --template vue
  ```

- **pnpm**:
  ```bash
  pnpm create vite my-vue-app --template vue
  ```

#### Testing the Project

Navigate to the project directory, install dependencies, and run the development server:

```bash
cd my-vue-app
npm install  
npm run dev  
```

Note: If components are written using the CommonJS specification, you need to additionally install the `vite-plugin-commonjs` plugin and make corresponding configurations to the `vite.config.js` file when using Vite.

### Import Operations

First, unzip the VisualLogic module zip package you need to import. Then, copy all the extracted files into a custom folder inside the `src` directory of the Vue project. Pay particular attention that if multiple VisualLogic modules are introduced in a project, the modules should be placed in different folders to avoid file overwriting conflicts.

### Component Integration

Implement the basic usage of the VisualLogic module and how to pass attributes in the Vite combined Vue environment.

#### Module Import

1. **Import Module Components**:
   Import the required VisualLogic module components in your Vue file:
   ```javascript
   import ivxMod from './components/ivxComponents';
   ```

2. **Define Module Property Variables**:
   Suppose the exported case module data includes `info` and `index` data, you need to define these data.
   ```javascript
   let mod_info = [
     {
       "name": "Base Information",
       "color": "#23C343",
       "showDot": 1
     },
     {
       "name": "Professional Internal Review",
       "color": "#C9CDD4",
       "showDot": 1
     },
     {
       "name": "Others",
       "color": "#FF7D00",
       "showDot": 1
     }
   ];
   let mod_index = 1;
   ```

3. **Insert Module and Bind Data in the Template**:
   Insert the module and bind the data in the `<template>`.
   ```html
   <template>
     <div>
       <a href="https://vitejs.dev" target="_blank">
         <img src="/vite.svg" class="logo" alt="Vite logo" />
       </a>
       <a href="https://vuejs.org/" target="_blank">
         <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
       </a>
     </div>
     <HelloWorld msg="Vite + Vue" />
     <!-- Add the ivx module and pass in properties -->
     <ivxMod :info="mod_info" :index="mod_index"></ivxMod>
   </template>
   ```

#### Module Method Invocation and Event Listening

1. **Module Method Invocation**:
   Declare and create a module ref pointing to the module instance, then construct a module calling function, and call it via `ref.value.methodName`, with the first parameter being the passing object and the second parameter being the callback function.

2. **Event Listening**:
   You can use `@` to bind events in the module instance. The name of the bound event is the name of the custom event in the VisualLogic editor module.

### Flowchart

```mermaid
graph TD
  A[Create Vue Project] --> B[Import VisualLogic Module]
  B --> C[Import Module Components]
  C --> D[Define Module Properties]
  D --> E[Insert Template and Bind Data]
  E --> F[Invoke Module Methods]
  F --> G[Listen to Module Events]
```

This is a detailed guide on how to import and use VisualLogic modules in a Vue project. Follow the steps to ensure correct import and usage of the modules.
 # React Module Import

### Overview

This article provides a detailed introduction on how to import and use the VisualLogic module in a React project. Below are the specific steps and operation guide.

### Environment Setup

On the Windows platform, use React-CLI for project construction. The initialization steps are as follows:

1. Create a new React project using React-CLI:
   ```shell
   npx create-react-app test-react
   ```
2. Start the development environment:
   ```shell
   cd test-react
   npm start
   ```

### Import Operation

1. Unzip the module zip package exported by VisualLogic.
2. Copy all the files from the unzipped folder into a custom folder within the `src` directory of the React project.
3. If multiple VisualLogic modules are introduced in a project, ensure that each module is placed in a different folder to avoid file overwrites and conflicts.

### Component Integration

Here is how to import and use the VisualLogic module in a React project:

1. **Import Module Component**  
   Import the module component in the React file; the component name should start with an uppercase letter:

   ```javascript
   import IvxMod from './ivxMod';
   ```
2. **Define Module Property Variables**  
   Define the property variables required by the module, such as the `info` and `index` data needed in the export example:

   ```javascript
   let mod_info = [
     {
       "name": "Basic Information",
       "color": "#23C343",
       "showDot": 1
     },
     {
       "name": "Professional Internal Audit",
       "color": "#C9CDD4",
       "showDot": 1
     },
     {
       "name": "Others",
       "color": "#FF7D00",
       "showDot": 1
     }
   ];
   let mod_index = 1;
   ```
3. **Use the Module in the Component Template and Bind Data**

   ```jsx
   <div className="App">
     <header className="App-header">
       <img src={logo} className="App-logo" alt="logo" />
       <p>Edit <code>src/App.js</code> and save to reload.</p>
       <a className="App-link" href="https://reactjs.org" target="_blank" rel="noopener noreferrer">
         Learn React
       </a>
       {/* Add VisualLogic module component and pass in props */}
       <IvxMod info={mod_info} index={mod_index}></IvxMod>
     </header>
   </div>
   ```

### Invoke Module Methods

1. Create a module `ref` and point to the module instance.
2. Build the module invocation function, using `ref.current` to call module methods. The first argument is the parameter object, and the second argument is the callback function.

### Listen to Module Events

1. In the module instance, bind module events using the format `on+EventName` where the event name should be the custom event name from the VisualLogic editor.

### Flowchart

```mermaid
graph TD
    A[Environment Setup] --> B[Unzip Module]
    B --> C[Import Files to src]
    C --> D[Import Module Component]
    D --> E[Define Property Variables]
    F[Bind Data to Template]
    E --> F
    F --> G[Create Module ref]
    G --> H[Invoke Module Methods]
    H --> I[Listen to Module Events]
```

By following the above steps, you can successfully import and use VisualLogic modules in a React project, achieving efficient component-based development.
 # Data aggregation generates statistical charts.

#### Overview

When summarizing data and generating statistical charts, we typically use VisualLogic tools to achieve this. The primary goal is to aggregate raw data into summary statistics, generating monthly or daily statistical data. The entire process can be implemented with different approaches depending on the data volume and real-time requirements.

#### Approach Overview

When performing data aggregation, you can choose the following two approaches:

- **Approach 1**: Perform calculations directly from the raw data table during real-time computation.
- **Approach 2**: Use an intermediate statistical table for real-time or scheduled statistics to reduce the pressure on the raw data table.

#### Approach 1: Real-Time Computation

This approach is suitable for situations with small volumes of raw data and infrequent statistical requests. Real-time data is fetched from the raw data table for calculations. However, this causes significant pressure on the database and server, making it unsuitable for scenarios with large data volumes or frequent requests.

- **Frontend Statistical Method**: Requires outputting data to the frontend and performing monthly statistics. This method needs to implement looped monthly statistics to avoid manual month-by-month calculations.
- **Backend Database Statistical Method**: Utilizes the database's "aggregate statistics" operations to automatically compute results. This method is more efficient with small data volumes.

```mermaid
graph LR
    subgraph Approach1
    A[Raw Data] -->|Continuous Write| B{Raw Data Table}
    B -->|Real-Time Calculation| C(Statistical Data)
    end
```

#### Approach 2: Periodic Aggregation for Large Data Volumes

When the data volume is very large (e.g., thousands or tens of thousands of entries per day), an intermediate approach is needed to aggregate raw data in real-time or on a schedule, allowing direct output of statistical data from the statistics table.

- **Real-Time Statistical Update**: Updates the statistics table with every new data entry. Suitable for scenarios with not too many statistics tables.
- **Scheduled Calculation Service**: Updates statistics through a scheduled backend service, only updating current month or day's data. This method conserves resources and is suitable for scenarios with low real-time requirements.

```mermaid
graph LR
    subgraph Approach2
    A[Raw Data] -->|Continuous Write| B{Raw Data Table}
    B -->|Real-Time Calculation| C{Statistical Data Table}
    C -->|Output| D(Statistical Data)
    C -->|Scheduled Calculation| B
    end
```

#### Implementation Flowchart

Below is a detailed flowchart for data aggregation and statistical chart generation using VisualLogic tools:

```mermaid
graph TD
    Start --> Collect Raw Data
    Collect Raw Data --> Assess Data Volume
    Assess Data Volume -->|Less than Threshold| Real-Time Calculation
    Real-Time Calculation --> Frontend Statistical Processing
    Real-Time Calculation --> Backend Categorized Statistics
    Assess Data Volume -->|Exceeds Threshold| Periodic Aggregation
    Periodic Aggregation --> Real-Time Statistical Update
    Periodic Aggregation --> Backend Scheduled Calculation
    Backend Scheduled Calculation --> Output Statistical Data
```

### Summary

Selecting the appropriate data processing method is crucial during data aggregation and statistical chart generation. Consider your data volume, real-time requirements, and resource usage for the most suitable implementation. Additionally, VisualLogic tools provide flexible components and structures, making this process more efficient and easier to maintain.
 # 5.0 Update Features

### Backend Java Source Code Export

VisualLogic 5.0 supports the export of Java source code based on the Spring Boot architecture. Users can export complete project code for compilation without the need for any foundational programs. Particularly in the microservices architecture that uses the Dubbo RPC framework, cross-service global transactions will be supported in the future. This feature can be accessed from the file menu under the export complete Java project option. For detailed compilation and deployment methods after export, please refer to the documentation: https://doc.ivx.cn/?page=18ebada5fec

### Callback Removal

In the new version, the callback mechanism has been removed, enhancing the synchronous execution functionality of the event panel. Now, event panels on both the frontend and backend can execute synchronously, allowing the reference of all returning results from the upper execution blocks. This enables the frontend and backend action groups' and functions' return results to be directly referenced in the formula editor.

### Formula Editor Optimization

VisualLogic 5.0 has optimized the formula editor by introducing preset input blocks such as color, string concatenation, conditional blocks, and value mapping, and supports nested formula editing. These enhanced features significantly improve the flexibility of formula editing and user experience.

### Conditional Block Optimization

Conditional blocks are optimized in the new version with added support for condition nesting. This update allows for more complex and refined logical expressions, helping developers to build more intelligent application logic.

### Local Variable Support in Event Panel

Event panels now support the definition of local variables internally. Users can add local variables on the right side of the event panel, enhancing the continuity and flexibility of event handling.

### Enhanced Loop Functionality

The loop mechanism now supports termination (break) and introduces array looping functionality. Users can directly select element values from each row of an array and set conditions to terminate the loop action, making loop processing more intuitive and efficient.

### New User Center

VisualLogic 5.0 introduces a new User Center feature, removing the traditional user and account system components. The User Center supports comprehensive user management functions, including user login, organization management, and permission management. By integrating a User Center, developers can implement all user-related features without developing user-related logic within the application.

### Frontend and Backend Automatic Log Support

Both the frontend and backend now support automatic log recording. Users can activate this feature by clicking the log icon on the right side of the event panel. Once activated, the frontend can view relevant information in the browser's inspection mode via the console, while the backend can view detailed information in logs.

### Image Repository Management

For enterprise users of the VisualLogic private deployment version, VisualLogic 5.0 allows users to push the Java source code deployment package to an image repository, supporting enterprise users' custom container cluster configurations and unified management within VisualLogic. This feature does not apply to individual and enterprise professional version users on the public network.

### New VisualLogic Model Template

Although this feature is not yet available, future updates will introduce model templates in VisualLogic to support visual data flow editing, simplifying the design process for developers.

### One-Click Import of Frontend npm Packages or UI Libraries

An upcoming feature will allow users to import npm packages or UI libraries with a single click, exclusive to the 5.0 version applications, significantly simplifying the steps and time for resource importation.

### Backend Support for Custom Interceptors

[Not yet available] The backend will support custom interceptors. Note: As the new version is still in the trial phase, certain features will be gradually improved in subsequent updates. Currently, the Java version of Socket and scheduled services is not supported.
 # Personal Free Version vs Professional Version

When using the VisualLogic development platform, users can choose between the personal free version and the professional version, which differ in terms of functionality. Here is a comparative analysis of the two:

##### Functionality Support

- **Personal Free Version Features**:

  - The personal free version only supports some of VisualLogic’s features. These features are suitable for entry-level users, allowing them to experience the basic graphical programming capabilities of VisualLogic.
  - In the free version, users cannot insert or export code. This means users cannot perform complex custom programming extensions and must rely solely on the components and features provided by VisualLogic.
  - The free version does not support private deployment, so users can only develop and test in the environment provided by VisualLogic.
- **Professional Version Features**:

  - Building on the features of the free version, the professional version extends and enhances the feature set, providing developers with a more comprehensive set of tools.
  - The professional version allows users to export full-stack code, which is especially important for teams looking to migrate projects from the VisualLogic environment to other platforms.
  - Users can write custom functions, such as performing more complex operations by handwriting JavaScript code.
  - The professional version supports SDK import, facilitating the invocation of backend code libraries, greatly enhancing the possibilities of backend development.
  - Developers can use the DBO (handwritten SQL) feature to gain greater flexibility and control over data processing and queries.

##### How to Unlock All Features

- To unlock all features of VisualLogic, users need to purchase the VisualLogic professional version. Purchasing the professional version can greatly expand development capabilities, and by using the professional version SN code, users can activate all features of the local IDE.
- If users have already purchased the VisualLogic professional version, the features of that version will be automatically activated when logging into the local version of the IDE.

When choosing a version of VisualLogic, users should weigh their options based on their needs and the complexity of their projects. For development teams or individuals requiring greater flexibility and functionality, the professional version provides a solution to overcome the limitations of the free version.