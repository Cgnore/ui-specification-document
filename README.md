# User Management Screen

## Overall

-  Displaying the user table.

-  Manipulating the visibility of the user table by filtering and toggling.

-  Disabling the user without hard delete.

-  Adding a new user to the system.

-  Editing the user's information in the platform.

## Initial State

When the page opens:

- In table, while the information of users is loading from the database, in the middle of the table there should be a 'spinner' icon to show the user that the information is loading.

- After fetching the records from the database table will be display the datas of user in ascending order by ID.

- The form will be displayed as empty.

- 'Hide Disabled User' toggle will be selected by default.

- 'New User' button will be active

- 'Save User' button will be disabled until the form is filled with specific user information such as User Name, Phone, Email and User Roles. <a id="assumption" name="assumption">&zwj;</a>(_Assumption: Because of there is no specification in the task assignment, the given user information are assumed as mandatory to provide reliable system._)

For clear understanding of the initial page status check the **flowchart** below:

```mermaid
graph TD
    Start([User Navigates to User Management Screen]) --> InitState[Initialize UI States]

    subgraph Default_Component_States [Default UI Component States]
        direction TB
        Form[Right Panel: Form is Displayed Empty]
        Toggle[Header: 'Hide Disabled User' Toggle is Checked]
        BtnNew[Header: '+ New User' Button is Enabled]
        BtnSave[Bottom: 'Save User' Button is Disabled]

    end

    InitState --> Default_Component_States
    InitState --> TableLoading[Left Panel: Show Loading Spinner in Table Grid]

    TableLoading --> DataResponse{Data Fetching State}

    DataResponse -- Success (Data Exists) --> RenderTable[Remove Spinner & Populate Table Rows]
    DataResponse -- Success (No Data) --> EmptyTable[Remove Spinner & Show 'No Users Found' Empty State]
    DataResponse -- Error --> ErrorState[Show Error Toast Notification & Empty Table]

    RenderTable --> Ready([UI Ready for Interaction])
    EmptyTable --> Ready
    ErrorState --> Ready
```
## Page Features

This section defines the core layout, styling, and structural components of the page. *(Hex codes are used as primary color specifications).*

**Global Styling:**
- **Page Background:** `#ffffff`
- **Spacing:** Responsive `space-between` applied between all main units.

---

### 1. Component Structure

Below is the top-level component hierarchy for the layout. The page consists of three main UI units.

```jsx
<UserManagementPage>
  <Header />       {/* Background: #f5f5f5 | Font-weight: bold | Contains 3 sub-units */}
  <UserTable />
  <UserForm />
</UserManagementPage>
```

---

### 2. Initial States & Rules

The table below defines the default states, visual behaviors, and specific validation/API rules for the core components upon initial load.

| Component | Initial State | Notes / API Rules |
| :--- | :--- | :--- |
| **Header Sub-units** | Visible | Text must be **bold**. Responsive spacing applied between the 3 sub-units. |
| **User Table** | `[]` (Empty) / Loading | Fetches initial user list from API on mount. |
| **User Form Fields** | Empty / Untouched | Default state for all input fields. Validation triggers on user interaction. |
| **Save User Button** | Disabled | Requires *Username*, *Phone*, *Email*, and *Roles* to be filled and valid. |


## Page Features
(_Because of there is no specification about primary color etc., hex code will be used in this document_)
Page main features will be determined in this section.
- Page background will be '#ffffff' hex code.
- The page will consist of three main UI units.
- The units are labelled as "Header", "Table" and "Form" in this document.
- The space-between units will be responsive.

The details for each components will be given in below.

### 1. Header

- The Header unit will be located on the top of the page.
- The header background will be '#f5f5f5' hex code.
- The header of the page contains three units which they will be explained more in related sections below. The space-between will be responsive.
- The text in the header units will be bold. 

There are three units of the header. The units are:

#### 1.1 New User Button
- The New User Button will be on the left side of the header.
- The button background will be '#2C74B3' hex code.
- The text colour will be '#FFFFFF' hex code.
- When the user hover the button, the background of the button will be '#20527D' hex code.
- The initial state of the button will be active.

Detailed explanation of the New User Button will be explained [here.](#new-user)

#### 1.2 Hide Disabled Checkbox
- Right after the New User button, the Hide Disabled User Checkbox will come.
- The text colour will be '#333333' hex code.
- The initial state of the checkbox will be ticked.

Detailed explanation of the Hide Disabled Checkbox will be explained [here.](#hide-disabled-checkbox)

#### 1.3 Save User Button
- The Save User Button will be on the right side of the header.
- The button background will be '#2C74B3' hex code when the button is active.
- While the button is active if the user hover the button, the background of the button will be '#20527D' hex code.
- The initial state of the button will be disabled.
- While the button is disabled the background will be in '#CCCCCC' hex code and cursor will not be allowed.

Detailed explanation of the Save User Button will be explained [here.](#save-user)

### 2. Table

The table will display the user information in a list row by row.

- The Table unit will be located on the left site on the page below the header.
- Its height will stretch to fill the remaining screen height (with a vertical internal scroll if needed).
- It will have 4 columns.
- The columns will be separated by lines which have the colour of '#E5E5E5' hex code.
- Inside the table, the 'Segoe UI' text font will be used.
<a id="hide-disabled-checkbox" name="hide-disabled-checkbox">&zwj;</a>

#### Hide/Disabled Checkbox

In the header of the page, there will be a checkbox to hide disabled user from the view. 
- If the user make the checkbox active, then the records of the disabled user will be invisible.
- If the user make the checkbox inactive, then all the data of the users will be shown in the table no matter of enable status of the records.

To move back to the header part of the document, click [here.](#12-hide-disabled-checkbox)


The table consist of two main parts. The parts of the table are "Table Header" and "Table Body". More detailed explanation will be handed in below:

### 2.1 Table Header

The Table Header will display the titles and features manipulating options to table view.
- The Table Header will be sticky.
- The background of the table header will be '#2C74B3' hex code.
- The text in the header will be '#FFFFFF' hex code.
- The text in the header will be bold. 

In each column of the header, there will be title and symbols.

#### 2.1.1 General Table Header Behaviours

##### Alignment Rules
- The text of the header will always be left-aligned.
- Icons always will be right-aligned.

##### Icon Alignment Rules
- The Filter Icon will always be on right.
- The Sort Icon will always be left side of the Filter Icon.

##### Functions of The Icon When Interact
- When user cilck the Filter Icon, there will be a dropdown list. This list gives the user the ability of selecting specific parameters to filter according column data. 
- When user click the Sort Icon, the table vision will be changed by ascending/descening order in alphabetic or numerical(depending on the column type).

##### Column Specifications

| Column Name | Sortable (Arrow Icon) | Filterable (Funnel Icon) | Data Type |
| :--- | :---: | :---: | :--- |
| **ID** | Yes | Yes | Numeric |
| **User Name** | Yes | Yes | Alphabetic |
| **Email** | Yes | Yes | Alphabetic |
| **Enabled** | Yes | Yes | Boolean (Status) |


### 2.2 Table Body

- The Table Body will display the user information.
- The Table Body will be vertical overflow with a scrollbar.
- The data which is displayed in each row will correspond to its column title.
- The background of the Table Body will be '#FFFFFF' hex code.
- The colour of the text in the Body will be '#333333' hex code.
- The rows will be clickable. To inform user, the row background will be '#F5F5F5' hex code while hovering.
- The text in the body will be regular boldness.

Each column, different details will be displayed.
- In the first column, the user ID will be placed right-aligned.
- The second column will display the User Name by left-aligned.
- The email details of the user will be located in the third column of the Table Body.
- The fourth column will display the user Enable status.

#### 2.2.1 Modifying User Information

The table gives ability to the user to select one of the user by clicking once and change that user's records through the form. The process of the modification user's records are:


- Select the appropriate user by clicking once.
- The row of the selected user will be highlighted by changing the background of that row.
- The background colour of that row will be '#BDD3E5' hex code.
- The form which is located in the right side of the table, will be populated with the records of the selected user.
- When the end-user change any records of the selected user, the "Save User" Button will be active and clickable.
- After the changes are made the user will click the save user button.
- When changes made and saved succesfully, the Table refreshes (spinning icon displayed in the middle of the table with a message to inform the user about refreshing) to display the latest parameters and the selected row stays highlighted.

To cancel the modifying function, highlighted row will be clickable. The cancel process is explained below:
- When user hover the mouse on the highlighted row, the background of the row will be '#F5F5F5' hex code.
- When user click the highlighted row, the background will be '#FFFFFF' hex code.
- When the row is deselected the form will be cleared also. 

  <br><br>
```mermaid
graph TD
    %% User Actions from Inside the Table    
    ActionEdit([User Clicks a Row in the Table]) --> HighlightRow[Row Background Changes to Active State]
    HighlightRow --> PopulateForm[Right Form Populates with Selected User's Data]
    
    PopulateForm --> FormInteraction{Does User Modify Any Data?}
    
    %% Save Button Activation Logic
    FormInteraction -- No (Pristine State) --> KeepSaveDisabled['Save User' Button Remains Disabled]
    FormInteraction -- Yes (Dirty State) --> ActivateSave['Save User' Button Becomes Enabled]
    
    %% Save and Refresh Process
    ActivateSave --> ClickSave([User Clicks 'Save User' Button])
    ClickSave --> ShowLoading[Show Loading Spinner on Table/Button]
    ShowLoading --> RefreshTable[Table Refreshes with Updated Data & Row Remains Highlighted]
  ```

### 3. Form

The form will include input fields, dropdown and checkbox. In these fields, user will be able to enter various user information to save the user information to the system through the form. 

In the header, there are two buttons to affect this section. The buttons functionality are explained below more:
<a id="new-user" name="new-user">&zwj;</a>

#### New User Button

- The new user button will erase all the inputs of the form to initial value, which is empty.
- The new user button always will be active to give the ability to end-user to clear the form.

To move back to the header part of the document, click [here.](#11-new-user-button)
<a id="save-user" name="save-user">&zwj;</a>
#### Save User Button

- The save user button will be active if the form filled with mandatory fields.
- Clicking the save user button will trigger the save process.
- While the saving process is continue, the form inputs will be disabled and the status of the button will turn disabled and a spinning icon will displayed in the middle of the button.
- If the parameters are saved successfully (according to the system response), the form will be cleaned.
- If the saving process has succeed, then form will be cleared and a toast success message will appear to make sure the user that the process is done successfully.
- If saving process has failed due to API response, then the form will be stayed with the last inputs and a toast error message will appear to explain and make user aware of that error.
- 
To move back to the header part of the document, click [here.](#13-save-user-button)

In the form, there will be four input fields whicil allows the user to enter data with max 255 characters. There will be a '*' sign in mandatory fields (Username, Phone, Email, User Roles according to the [assumption](#assumption)).

- **Username (*)**: Text input.
- **Display Name**: Text input.
- **Phone (*)**: Numeric input. Only digits and the '+' sign allowed.
- **Email (*)**: Text input. Standard email validation rules have been applied.
- **User Roles (*)**: Dropdown with availible roles ('Guest', 'Admin' and 'SuperAdmin')
  - The initial state of the dropdown is showing a message to user which is 'Select user roles...'
  - When user click the box, the roles dropdown will be opened.
  - The initial state for the first opening of the dropdown, the 'Guest' option will be highlighted.
  - The user will be able to change the highlighted role through the mouse point and through the up and down arrows in the keyboard.
- **Enabled**: The checkbox will allow the user to change the status of the user to be active or disabled.
  - Initial State: Unchecked
  - Interaction: Ticked = Active, Unticked = Disabled. 








