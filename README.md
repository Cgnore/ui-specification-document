# User Management Screen

## 1) Core Features

-  User List.

-  Filtering/Toggling the List.

-  User Creation & Modification by Form.

## 2) Initial State

When the page opens, the components will be rendered with the following default states and rules:

| Component | Initial State | Details / Validation |
| :--- | :--- | :--- |
| **User Table (Loading)** | Displays Spinner | A spinner icon is shown in the middle of the table. |
| **User Table (Loaded)** | Ascending Order | Once loaded, the table displays user data ordered by ID in ascending order by default. |
| **User Form** | Empty | All input fields in the form are completely empty. |
| **'Hide Disabled User' Toggle** | Ticked | Disabled users hid by default. |
| **'New User' Button** | Active | Ready to be interacted. |
| **'Save User' Button** | Disabled | Remains disabled until *User Name, Phone, Email,* and *User Roles* are filled. <br><br><a id="assumption" name="assumption">&zwj;</a>*(_Assumption: Because of there is no specification in the task assignment, the given user information are assumed as mandatory to provide reliable system._)* |

### Initial State Flowchart

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

## 3) Page Features

- **Page Background:** `#ffffff`
- **Layout:** Consists of three main UI units ('Header', 'Form', and 'Table').
- **Spacing:** The space-between units will be responsive.

### 1. Header

- **Position:** Located on the top of the page.
- **Background:** `#f5f5f5`
- **Text Features:** All text in the header units will be **bold**.
- **Layout:** Contains three sub-units with responsive space-between.

#### Header Components

| Component | Position | Initial State | Background (Default/Hover/Disabled) | Text Color | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **New User Button** | Left | Active | `#2C74B3` / `#20527D` / - | `#FFFFFF` | See detailed explanation [here.](#new-user) |
| **Hide Disabled Checkbox** | Middle (Next to New User) | Ticked | - | `#333333` | See detailed explanation [here.](#hide-disabled-checkbox) |
| **Save User Button** | Right | Disabled | `#2C74B3` / `#20527D` / `#CCCCCC` | - | `cursor: not-allowed` when disabled. See detailed explanation [here.](#save-user) |

### 2. Table

**Table & Layout Attributes**
- **Layout:** Located on the left side under the header, stretches to fill the remaining screen height (with internal vertical scrolling).
- **Columns:** 4
- **Styling:** Font: 'Segoe UI', Column Separator / Border Color: `#E5E5E5`
<a id="hide-disabled-checkbox" name="hide-disabled-checkbox">&zwj;</a>
#### Hide/Disabled Checkbox

Located in the header, this checkbox toggles the visibility of disabled user accounts in the table.

<details>
  <summary><strong>Filter Logic: Hide Disabled Users</strong></summary>
  
  <br>

  * **State:** Bound to a local state (e.g., `isHideDisabledActive`) with an **initial state = `true`**.
  * **When `true` (Active):** The user array fetched from the database is filtered before rendering. Any user object containing an `isDisabled === true` (or equivalent) flag will not be rendered in the DOM.
  * **When `false` (Inactive):** The complete user list is mapped and rendered directly without any filtering manipulation, showing all users regardless of their status.
  
  <br>
  
  [↑ Return to Header Components](#header-components)
</details>

### 2.1 Table Header

The Table Header displays column titles and gives the user the ability to manipulate the view.

**Styling & Layout:**
- **Position:** `sticky` | **Top**
- **Background:** `#2C74B3`
- **Text:** `#FFFFFF` | **Bold**

> **Header Cell Layout:** Uses flexbox alignment. The header text is always left-aligned, while the action icons are right-aligned. The **Sort Icon** is always placed to the left of the **Filter Icon**.

**Icon Actions:**
* **Sort Icon (`onClick`):** Toggles the table view between ascending / descending order (alphabetical or numerical, depending on the column's data type).
* **Filter Icon (`onClick`):** Opens a dropdown menu. Allowing the user to select specific parameters to filter the respective column data.

**Column Specifications:**

| Column Name | Sortable (Arrow Icon) | Filterable (Funnel Icon) | Data Type |
| :--- | :---: | :---: | :--- |
| **ID** | Yes | Yes | Numeric |
| **User Name** | Yes | Yes | Alphabetic |
| **Email** | Yes | Yes | Alphabetic |
| **Enabled** | Yes | Yes | Boolean |

### 2.2 Table Body

- The Table Body will be vertical overflow with a scrollbar.
- The background of the Table Body will be '#FFFFFF' hex code.
- The colour of the text in the Body will be '#333333' hex code.
- The rows will be clickable. The row background will be '#F5F5F5' hex code while hovering.
- The text in the body will be regular boldness.

| Column Name | Alignment |
| :--- |  :--- |
| **ID** | Right |
| **User Name** | Left |
| **Email** | Left |
| **Enabled** | Left |

#### 2.2.1 Row Selection & User Interaction

- Select the appropriate user by clicking once.
- The row of the selected user will be highlighted by changing the background of that row.
- The background colour of that row will be '#BDD3E5' hex code.
- The form which is located in the right side of the table, will be populated with the records of the selected user.
- When the end-user change any records of the selected user, the "Save User" Button will be active and clickable.
- After the changes are made the user will click the save user button.
- When changes made and saved succesfully, the Table refreshes (spinning icon displayed in the middle of the table with a message while refreshing) to display the latest parameters and the selected row stays highlighted.

- Highlighted row will be clickable.
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
```mermaid
    stateDiagram-v2
    [*] --> UnselectedRow
    UnselectedRow --> SelectedRow : onClick (Bg: #BDD3E5) \n Action: Populate Form
    SelectedRow --> UnselectedRow : onClick (Bg: #FFFFFF) \n Action: Clear Form
    SelectedRow --> Updating : Form Changed -> Save Clicked
    Updating --> SelectedRow : API Success \n Action: Table Refresh (Spinner)
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








