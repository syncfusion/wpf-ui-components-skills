# OLAP Client Elements and UI Components

## Table of Contents
- [Overview](#overview)
- [Cube Selector](#cube-selector)
- [Cube Dimension Browser](#cube-dimension-browser)
  - [Types of Nodes](#types-of-nodes)
  - [Attribute Hierarchy](#attribute-hierarchy)
  - [User-Defined Hierarchy](#user-defined-hierarchy)
  - [Differentiating Hierarchies](#differentiating-hierarchies)
  - [Symbolic Representation of Nodes](#symbolic-representation-of-nodes)
- [Axis Element Builder](#axis-element-builder)
  - [Categorical (Column) Axis](#categorical-column-axis)
  - [Series (Row) Axis](#series-row-axis)
  - [Slicer Axis](#slicer-axis)
  - [Adding Elements to an Axis](#adding-elements-to-an-axis)
  - [Removing Elements from an Axis](#removing-elements-from-an-axis)
  - [Rearranging Elements in an Axis](#rearranging-elements-in-an-axis)
- [Elements Editor](#elements-editor)
  - [Measure Editor](#measure-editor)
  - [Member Editor](#member-editor)
- [Toolbar](#toolbar)
- [Report Manipulation](#report-manipulation)
  - [New Report](#new-report)
  - [Add Report](#add-report)
  - [Remove Report](#remove-report)
  - [Rename Report](#rename-report)
  - [Save Report](#save-report)
  - [Load Report](#load-report)
  - [Report List](#report-list)
- [OLAP Grid and OLAP Chart](#olap-grid-and-olap-chart)

## Overview

The OLAP Client consists of multiple UI elements that work together to provide a comprehensive business intelligence experience. This guide covers all major UI components and their interactions.

## Cube Selector

The cube selector allows you to select any one of the cubes available in the connected database. This is presented as a drop-down list displaying all cube names. When you select a cube from the drop-down list, the corresponding cube elements are loaded into the dimension browser and become available for analysis.

### Use Cases

- **Multi-Cube Databases**: Switch between different analytical perspectives (Sales, Inventory, HR, etc.)
- **Cube Comparison**: Compare similar data across different cube structures
- **Dynamic Analysis**: Change analytical focus without reconnecting to the database

## Cube Dimension Browser

The cube dimension browser is a tree view-like structure that organizes the cube elements such as dimensions, hierarchies, and measures from the selected cube into independent logical groups. This provides an intuitive interface for browsing and selecting elements to build your analysis.

### Types of Nodes

| Node Type | Description | Purpose |
|-----------|-------------|---------|
| **Display Folder** | A folder that contains a set of similar elements | Organizational grouping |
| **Measure** | Quantity available for analysis | Quantitative data for calculations |
| **Dimension** | A name given to parts of the cube that categorize data | Data categorization |
| **Attribute Hierarchy** | Level of attributes down the hierarchy | Flat hierarchy with one level |
| **User-Defined Hierarchy** | Members of a dimension in a hierarchical structure | Multi-level navigation path |
| **Level** | Denotes a specific level in the category | Specific depth in hierarchy |
| **Named Set** | A collection of tuples and members defined in the cube | Predefined member groups |

### Attribute Hierarchy

An attribute hierarchy contains the following structural levels:

- **Leaf Level**: Contains distinct attribute members, where each member is known as a leaf member
- **Intermediate Levels**: Exist if the attribute hierarchy is a parent-child hierarchy (optional)

**Characteristics:**
- Single-level structure (typically just the leaf level)
- Automatically created for each attribute
- Flat organization without parent-child relationships

### User-Defined Hierarchy

A user-defined hierarchy organizes the members of a dimension into a hierarchical structure and provides navigation paths in a cube.

**Example:**  
A dimension table with attributes such as **Year**, **Quarter**, and **Month** can be organized into a user-defined hierarchy named **Calendar** in the Time dimension, providing navigation across all time levels.

**Characteristics:**
- Multi-level structure with related attributes
- Provides drill-down navigation paths
- Explicitly designed for analytical purposes
- Common examples: Geography (Country → State → City), Time (Year → Quarter → Month)

### Differentiating Hierarchies

| Feature | User-Defined Hierarchy | Attribute Hierarchy |
|---------|----------------------|-------------------|
| **Levels** | More than one level | Only one level |
| **Purpose** | Navigation between related attributes | Access to single attribute members |
| **Structure** | Hierarchical with parent-child relationships | Flat list of members |
| **Creation** | Explicitly defined in cube design | Automatically created per attribute |

### Symbolic Representation of Nodes

Each node type in the cube dimension browser has a distinct icon:

| Name | Node Type | Is Draggable |
|------|-----------|-------------|
| Display Folder | Display Folder | False |
| Measure | Measure | **True** |
| Dimension | Dimension | **True** |
| User Defined Hierarchy | Hierarchy | **True** |
| Attribute Hierarchy | Hierarchy | **True** |
| Levels (in order) | Level Element | **True** |
| Named Set | Named Set | **True** |

**Note:** Only draggable elements (marked as True) can be used to build reports by dragging them to the axis element builder.

## Axis Element Builder

The axis element builder allows you to build elements in the axes of the OLAP Client. This supports three axes: **Categorical**, **Series**, and **Slicer**. Based on the construction of these axes, the OLAP Grid and OLAP Chart will display the resultant data.

### Categorical (Column) Axis

The categorical axis defines one or more elements that are displayed:
- Along the **chart's y-axis** as labels
- In the **columns of the grid**

**Stacking Behavior:**  
If more than one dimension is present on the categorical axis, the chart/grid will stack each dimension. The stacking order is based on the order that dimensions appear on the categorical axis.

**Use Cases:**
- Product categories across columns
- Time periods as column headers
- Geographic regions as column breakdowns

### Series (Row) Axis

The series axis defines one or more dimensions that are displayed:
- Along the **chart's x-axis** as labels
- In the **rows of the grid**

**Stacking Behavior:**  
If more than one dimension is present on the series axis, the chart or grid will stack each dimension. The stacking order is based on the order that dimensions appear on the series axis.

**Use Cases:**
- Measures as row items
- Customer segments across rows
- Product lines as row categories

### Slicer Axis

The slicer axis is used as a **filter** to narrow the focus of the multidimensional data displayed in the chart or grid. It allows you to analyze a member of the dimension in depth.

**Important Rule:**  
To display a member's data in the slicer, the corresponding member **must not** be present on both the categorical axis and series axis simultaneously.

**Use Cases:**
- Filter by specific year without showing it in rows/columns
- Focus analysis on particular region
- Narrow scope to specific product category
- Apply context filters that don't need visual representation

### Adding Elements to an Axis

Dimensions, measures, hierarchies, levels, and named set elements can be added to any axis using **drag-and-drop** operations:

1. Locate the element in the Cube Dimension Browser
2. Click and hold the element (must be draggable - see icon table above)
3. Drag the element to the desired axis in the Axis Element Builder
4. Drop the element at the desired position

**Positioning:**  
Elements can be dropped between existing elements to control their order, which affects stacking and display sequence.

### Removing Elements from an Axis

Two methods are available for removing elements:

**Method 1: Delete Icon**
1. Hover over the element in the axis
2. Click the **delete icon** that appears

**Method 2: Context Menu**
1. Right-click on the element
2. Select **Remove** from the context menu

### Rearranging Elements in an Axis

Change the order of elements within an axis to adjust stacking and display sequence:

1. Hover over the element you want to move
2. Click **Move Up** icon to move the element higher in the order
3. Click **Move Down** icon to move the element lower in the order

**Why Reorder?**  
Element order affects:
- Stacking hierarchy in charts
- Row/column grouping in grids
- Visual precedence in the display

## Elements Editor

The Elements Editor dialogs provide detailed selection and configuration for measures and members.

### Measure Editor

The measure editor is a dialog that displays the **collection of measures** in the current report. It provides an interface for selecting which measures to include in your analysis.

**Opening the Measure Editor:**
- Click the **split button** at the right corner of the measure node in the axis element builder


**Features:**
- View all available measures
- Select/deselect measures for the report
- Quick access to multiple measure selection

### Member Editor

The member editor is a dialog that displays the **members of the current hierarchy** in a tree view structure. This allows fine-grained control over which members are included in the analysis.

**Opening the Member Editor:**
- Click the **split button** at the right corner of the member node in the axis element builder

**Features:**
- Tree view of all hierarchy members
- **Check All** option: Select all members at once
- **Uncheck All** option: Deselect all members at once
- Individual member selection via checkboxes
- Hierarchical member browsing

**Use Cases:**
- Filter specific countries from a geography dimension
- Select only certain months from a time hierarchy
- Exclude outlier members from analysis
- Build focused reports with selected members only

## Toolbar

The OLAP Client toolbar provides quick access to common operations:

### Toolbar Options

| Icon/Button | Name | Description |
|-------------|------|-------------|
| 🔌 | **Connect to server** | Connect data source via offline cube, online server, or connection string |
| 📄 | **New report** | Create a new report list, clearing existing collection |
| 📂 | **Load report** | Load a saved report collection from the local system |
| 💾 | **Save report** | Store the report collection in the local system |
| 💾➕ | **SaveAs report** | Store a copy of the report collection with a new name |
| ➕ | **Add report** | Add a new report to the existing list |
| ➖ | **Remove report** | Remove the current report (if more than one report exists) |
| ✏️ | **Rename report** | Change the name of the current report |
| 🔄 | **Toggle axis** | Interchange items between categorical and series axes |
| 🔍 | **Show expander** | Display expander option for drill-down operations |
| 🔽📊 | **Filter/sort column** | Filter or sort data with respect to the column |
| 🔽📈 | **Filter/sort row** | Filter or sort data with respect to the row |
| 📋 | **Report list** | Drop-down of all reports in current session |
| 📜 | **MDX query** | Display the executed MDX query for current data |

## Report Manipulation

The toolbar provides comprehensive report management capabilities.

### New Report

Creates a new report collection with a single empty report, **clearing the existing report collection**.

**Steps:**
1. Click the **New Report** icon on the toolbar
2. Enter a name for the report in the dialog
3. Click **OK** to create (or **Cancel** to abort)
4. The report collection is reset with only the new report

**Use Case:** Start fresh analysis from scratch

### Add Report

Adds a new report to the **existing** report collection (does not clear other reports).

**Steps:**
1. Click the **Add Report** icon on the toolbar
2. Enter a name for the new report
3. Click **OK** to add (or **Cancel** to abort)
4. The new report is added to the collection

**Use Case:** Build multiple analytical perspectives in one session

### Remove Report

Removes the current/active report from the report collection.

**Requirement:** Works only if the report collection has **more than one report**. You cannot remove the last remaining report.


**Use Case:** Clean up unwanted reports while keeping others

### Rename Report

Changes the name of the current/active report.

**Steps:**
1. Click the **Rename Report** icon on the toolbar
2. Enter a new name in the dialog
3. Click **OK** to apply (or **Cancel** to abort)
4. The active report is refreshed with the new name

### Save Report

Saves the current report collection to the local system as a file.

**Steps:**
1. Click the **Save Report** icon
2. The SaveAs file dialog opens
3. Choose location and provide a file name
4. Click **Save**
5. The report is stored at the selected location

**File Format:** Typically saved as XML containing report structure

### Load Report

Loads a previously saved report from the local system.

**Steps:**
1. Click the **Load Report** icon on the toolbar
2. The Open file dialog appears
3. Navigate to and select the saved report file
4. Click **Open**
5. The report is loaded into the OLAP Client

### Report List

The report list drop-down contains the names of all reports in the current report collection.


**Usage:**
1. Click the report list drop-down
2. Select the desired report from the list
3. The selected report becomes active and loads immediately

**Use Case:** Quick switching between multiple analytical views in one session

## OLAP Grid and OLAP Chart

The [OLAP Grid](http://help.syncfusion.com/wpf/olapgrid/overview/) and [OLAP Chart](http://help.syncfusion.com/wpf/olapchart/overview) controls are the primary visualization components that render data based on the operations performed in the Axis Element Builder.

**Synchronization:**  
Both controls automatically update when you:
- Add/remove elements from axes
- Change element order
- Apply filters
- Modify sorting
- Switch cubes

**Display Modes:**
- Grid only
- Chart only
- Both Grid and Chart simultaneously

**Integration:**  
The Grid and Chart work together to provide both tabular and graphical representations of the same multidimensional data, giving users flexibility in how they analyze and present information.

## Next Steps

Now that you understand the OLAP Client UI elements:

1. Learn about filtering data - see [filtering.md](filtering.md)
2. Explore sorting capabilities - see [sorting.md](sorting.md)
3. Understand report management - see [report-management.md](report-management.md)
