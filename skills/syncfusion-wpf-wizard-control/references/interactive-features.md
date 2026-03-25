# Interactive Features in WPF WizardControl

## Table of Contents
- [Populating Pages Declaratively](#populating-pages-declaratively)
- [Populating Pages via Data Binding](#populating-pages-via-data-binding)
- [Selecting a Page Programmatically](#selecting-a-page-programmatically)
- [Title and Description on WizardPage](#title-and-description-on-wizardpage)
- [Non-Linear Navigation (NextPage / PreviousPage)](#non-linear-navigation-nextpage--previouspage)
- [Next Button Event Handler](#next-button-event-handler)
- [Common Patterns](#common-patterns)

---

## Populating Pages Declaratively

The simplest approach — declare `WizardPage` elements directly inside `WizardControl` in XAML.
Use this when the number and content of pages are fixed at design time.

```xaml
<syncfusion:WizardControl Name="wizardControl">
    <syncfusion:WizardPage Name="wizardPage1" Title="Step 1"/>
    <syncfusion:WizardPage Name="wizardPage2" Title="Step 2"/>
    <syncfusion:WizardPage Name="wizardPage3" Title="Step 3"/>
</syncfusion:WizardControl>
```

```csharp
WizardControl wizardControl = new WizardControl();

WizardPage page1 = new WizardPage();
WizardPage page2 = new WizardPage();
WizardPage page3 = new WizardPage();

wizardControl.Items.Add(page1);
wizardControl.Items.Add(page2);
wizardControl.Items.Add(page3);
```

---

## Populating Pages via Data Binding

Use `ItemsSource` to drive wizard pages from a ViewModel collection. This is ideal for dynamic
wizards where the page count or content changes at runtime.

### Step 1 — Define the Model

```csharp
public class WizardStepModel : INotifyPropertyChanged
{
    private string _title;
    public string Title
    {
        get => _title;
        set { _title = value; OnPropertyChanged(nameof(Title)); }
    }

    private string _content;
    public string Content
    {
        get => _content;
        set { _content = value; OnPropertyChanged(nameof(Content)); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### Step 2 — Populate the ViewModel

```csharp
public class WizardViewModel
{
    public ObservableCollection<WizardStepModel> Steps { get; } = new();

    public WizardViewModel()
    {
        Steps.Add(new WizardStepModel { Title = "Welcome",       Content = "Introduction text." });
        Steps.Add(new WizardStepModel { Title = "Configuration", Content = "Set up your options." });
        Steps.Add(new WizardStepModel { Title = "Finish",        Content = "Setup is complete." });
    }
}
```

### Step 3 — Bind in XAML

```xaml
<Window.DataContext>
    <local:WizardViewModel/>
</Window.DataContext>

<Window.Resources>
    <!-- Style applied to each auto-generated WizardPage container -->
    <Style x:Key="WizardPageStyle" TargetType="syncfusion:WizardPage">
        <Setter Property="Title"       Value="{Binding Title}"/>
        <Setter Property="PageType"    Value="Interior"/>
        <Setter Property="BannerImage" Value="Images/banner.png"/>
    </Style>
</Window.Resources>

<syncfusion:WizardControl ItemsSource="{Binding Steps}"
                          ItemContainerStyle="{StaticResource WizardPageStyle}">
    <syncfusion:WizardControl.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Content}" TextWrapping="Wrap" Margin="20"/>
        </DataTemplate>
    </syncfusion:WizardControl.ItemTemplate>
</syncfusion:WizardControl>
```

> **How it works:** `ItemsSource` generates one `WizardPage` per item. `ItemContainerStyle`
> styles each generated `WizardPage` (binds `Title`, sets `PageType`, etc.).
> `ItemTemplate` defines what's rendered inside the page body.

---

## Selecting a Page Programmatically

Use `SelectedWizardPage` to navigate to any page in code, bypassing the normal sequential flow.

**Bind in XAML** (use `ElementName` binding to reference a named page):
```xaml
<syncfusion:WizardControl Name="wizard"
                          SelectedWizardPage="{Binding ElementName=step2}">
    <syncfusion:WizardPage x:Name="step1" Title="Step 1"/>
    <syncfusion:WizardPage x:Name="step2" Title="Step 2"/>
    <syncfusion:WizardPage x:Name="step3" Title="Step 3"/>
</syncfusion:WizardControl>
```

**Set in C#:**
```csharp
// Navigate directly to the third page
wizard.SelectedWizardPage = step3;
```

> **Use case:** Jump to a specific page after a validation failure, or return the user to a
> previously completed step when they click an edit link in a summary page.

---

## Title and Description on WizardPage

Each page can display a `Title` and `Description` in its header area (visible on Interior and
Exterior page types).

```xaml
<syncfusion:WizardPage Name="detailsPage"
                       Title="Enter Your Details"
                       Description="Provide your name and contact information."
                       Foreground="Navy"
                       PageType="Interior"/>
```

```csharp
WizardPage detailsPage = new WizardPage();
detailsPage.Title       = "Enter Your Details";
detailsPage.Description = "Provide your name and contact information.";
detailsPage.Foreground  = Brushes.Navy;
detailsPage.PageType    = WizardPageType.Interior;
```

> **Note:** On `PageType="Blank"`, Title and Description are not shown in a banner header area.

---

## Non-Linear Navigation (NextPage / PreviousPage)

By default, Next navigates to the immediately following page and Back goes to the previous one.
Override this with `NextPage` and `PreviousPage` to create branching or skipping flows.

**Skip a step based on a previous selection:**
```xaml
<syncfusion:WizardControl Name="wizard">
    <!-- page1 skips page2 and goes straight to page3 -->
    <syncfusion:WizardPage x:Name="page1" Title="Welcome"
                           NextPage="{Binding ElementName=page3}"/>
    <syncfusion:WizardPage x:Name="page2" Title="Advanced Options"/>
    <!-- page3 going back returns to page1, not page2 -->
    <syncfusion:WizardPage x:Name="page3" Title="Summary"
                           PreviousPage="{Binding ElementName=page1}"/>
    <syncfusion:WizardPage x:Name="page4" Title="Finish"/>
</syncfusion:WizardControl>
```

**Set programmatically (e.g., based on a user choice):**
```csharp
private void OnAdvancedChecked(object sender, RoutedEventArgs e)
{
    // Show advanced options page next
    page1.NextPage = page2;
    page2.PreviousPage = page1;
    page3.PreviousPage = page2;
}

private void OnAdvancedUnchecked(object sender, RoutedEventArgs e)
{
    // Skip advanced options page
    page1.NextPage = page3;
    page3.PreviousPage = page1;
}
```

> **Gotcha:** If you set `NextPage` on a page, make sure the target page's `PreviousPage` is
> also updated — otherwise the Back button on the target page will go to the wrong place.

---

## Next Button Event Handler

Subscribe to the `Next` event on `WizardControl` to execute logic or validate the current page
before navigation occurs.

```xaml
<syncfusion:WizardControl Name="wizard" Next="Wizard_Next">
    <syncfusion:WizardPage Name="inputPage" Title="Enter Details"/>
    <syncfusion:WizardPage Name="summaryPage" Title="Summary"/>
</syncfusion:WizardControl>
```

```csharp
private void Wizard_Next(object sender, RoutedEventArgs e)
{
    var wizard = (WizardControl)sender;

    // Validate before allowing navigation
    if (wizard.SelectedWizardPage == inputPage)
    {
        if (string.IsNullOrWhiteSpace(NameTextBox.Text))
        {
            MessageBox.Show("Please enter your name before continuing.");
            e.Handled = true;  // Cancel the navigation
            return;
        }
    }

    // Optionally update summary page content before navigating
    if (wizard.SelectedWizardPage == inputPage)
    {
        summaryLabel.Text = $"Name: {NameTextBox.Text}";
    }
}
```

> **Cancelling navigation:** Set `e.Handled = true` to prevent the wizard from moving to the
> next page. The user stays on the current page.

---

## Common Patterns

### Wizard with Conditional Branch

```csharp
// When user selects "Custom" installation type, include the extras page
private void InstallTypeChanged(object sender, SelectionChangedEventArgs e)
{
    if (installTypeCombo.SelectedIndex == 1) // Custom
    {
        standardPage.NextPage = extrasPage;
        extrasPage.PreviousPage = standardPage;
        summaryPage.PreviousPage = extrasPage;
    }
    else // Typical
    {
        standardPage.NextPage = summaryPage;
        summaryPage.PreviousPage = standardPage;
    }
}
```

### Navigate Programmatically After Async Operation

```csharp
private async void Wizard_Next(object sender, RoutedEventArgs e)
{
    var wizard = (WizardControl)sender;
    if (wizard.SelectedWizardPage == downloadPage)
    {
        e.Handled = true;  // Don't auto-advance
        progressBar.Visibility = Visibility.Visible;
        await DownloadFilesAsync();
        progressBar.Visibility = Visibility.Collapsed;
        wizard.SelectedWizardPage = completePage;  // Advance manually
    }
}
```

### Populate from Database

```csharp
public async Task LoadWizardStepsAsync()
{
    var steps = await _repository.GetSetupStepsAsync();
    Steps.Clear();
    foreach (var step in steps)
        Steps.Add(new WizardStepModel { Title = step.Name, Content = step.Instructions });
}
```
