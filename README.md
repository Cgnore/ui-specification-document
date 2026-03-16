# User Management Screen

## Introduction



This is a document to define the specifications of the user management screen. This document provides:

- **Overall:** Defines the main purpose, goals of this page.

- **Initial State:** Shows the initial status of the page's components.

- **Components:** Explains each component's behaviours and what functions will be defined.



## Overall



This page is designed to manage users in the platform. The "User Management Screen" will be on **one page**. It will include features and these features will **only be visible and usable** for the **"SuperAdmin"** role. These features are:



-  Displaying the user table.

-  Manipulating the visibility of the user table by filtering and toggling.

-  Disabling the user without hard delete.

-  Adding a new user to the system.

-  Editing the user's information in the platform.



## Initial State



When the page opens:



- The user table will be displayed.
  
- While the information of users is loading from the database, in the middle of the table there should be a 'spinner' icon to show the user that the information is loading.

- The form will be displayed as empty.

- 'Hide Disabled User' toggle will be selected by default.

- 'New User' button will be active

- 'Save User' button will be disabled until the form is filled with any user information.




For clear understanding of the initial page status check the **flowchart** below:

```mermaid
graph TD
    %% Start and Authorization Check
    Start([Navigate to User Management Screen]) --> AuthCheck{Is User SuperAdmin?}
    AuthCheck -- No --> AccessDenied([Access Denied])
    AuthCheck -- Yes --> PageInit[Initialize Page]

    %% Initial Page Load State
    subgraph Initial_Page_State [Initial Page State]
        direction TB
        InitLoading[Show Loading Icon in Table]
        InitForm[Display Empty Form]
        InitToggle['Hide Disabled User' Toggle Selected]
        InitBtnNew['New User' Button Active]
        InitBtnSave['Save User' Button Disabled]
    end

    PageInit --> Initial_Page_State

    %% Fetching Data
    InitLoading --> FetchDB[(Fetch Data from Database)]
    FetchDB --> ShowTable[Remove Loading Icon & Display User Table]

    %% User Actions from Table
    ShowTable --> UserActions{SuperAdmin Actions}
    
    UserActions --> ActionFilter[Filter Table / Toggle Visibility]
    UserActions --> ActionAdd[Start Filling Empty Form - Add New User]
    UserActions --> ActionEdit[Click a User Row in Table - Edit/Disable User]

    %% Add New User Flow
    ActionAdd --> CheckFormFilled{Is Form Filled With Any Info?}
    
    %% Edit Existing User Flow
    ActionEdit --> HighlightRow[Clicked Row is Highlighted]
    HighlightRow --> PopulateForm[Form Populates with User Data]
    PopulateForm --> EditOptions{Edit Actions}
    
    EditOptions --> ModifyInfo[Modify User Information]
    EditOptions --> ToggleEnabled[Toggle 'Enabled' Switch inside Form - Soft Delete/Activate]
    
    ModifyInfo --> CheckChanges{Are There Any Changes Made?}
    ToggleEnabled --> CheckChanges

    %% Save Button Activation Logic
    CheckFormFilled -- No --> KeepSaveDisabled['Save User' Button Remains Disabled]
    CheckChanges -- No --> KeepSaveDisabled
    
    CheckFormFilled -- Yes --> ActivateSave['Save User' Button Becomes Active]
    CheckChanges -- Yes --> ActivateSave

    %% Save and Refresh Process
    ActivateSave --> ClickSave[Click 'Save User' Button]
    ClickSave --> UpdateDB[(Update Database)]
    
    %% Return to initial state after updating
    UpdateDB --> FetchDB
```

# Components
Details related to components
Behavior of the page commponents

**FEATURES**
- New User
- User Table (ID, user name, email, user ability[they are filtered])
- Hide Disabled User Feature
- Adding new user form (username, display name, phone, email, user roles[guest, admin, super admin], account ability, save user)

