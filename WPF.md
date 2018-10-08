
# WPF #

### XAML
#### Special Characters
Finalizados con punto y coma;
> 
- Less than (<) &lt
- Greater than (>) &gt
- Ampersand (&) &amp
- Quotation mark (“) &quot

XML collapses all whitespace, which means a long string of spaces, tabs,
and hard returns is reduced to a single space. If you may want to include a series of several spaces in your button text. In this case, you need to use the `xml:space=”preserve”` attribute on your element.

```xaml
<TextBox Name="txtQuestion" xml:space="preserve">
[There is a lot of space inside these quotation marks "  ".]</TextBox>
```

#### Layout ###
In WPF, layout is determined by the container that you use. Although there are several containers to choose from, the **ideal** WPF window follows a few key principles:
- Elements (such as controls) should not be explicitly sized. Instead, they grow to fit
their content. For example, a button expands as you add more text. You can limit
controls to acceptable sizes by setting a maximum and minimum size
- Elements do not indicate their position with screen coordinates. Instead, they are
arranged by their container based on their size, order, and (optionally) other
information that’s specific to the layout container. If you need to add whitespace
between elements, you use the Margin property

#### Using Types from Other Namespaces
**Naming Conventions, samples**

```xaml
 xmlns:Prefix="clr-namespace:Namespace;assembly=AssemblyName"

 xmlns:sys="clr-namespace:System;assembly=mscorlib"
 xmlns:local="clr-namespace:MyNamespace"
 ```

### Tips & Tricks

- **Get the XAML content from an external file**
```csharp
DependencyObject rootElement;
using (FileStream fs = new FileStream(xamlFile, FileMode.Open))
{
	rootElement = (DependencyObject)XamlReader.Load(fs);
}
```

- **Find the control by name**
```csharp
button1 = (Button)LogicalTreeHelper.FindLogicalNode(rootElement, "button1");
// Wire up the event handler.
button1.Click += button1_Click;
```

Another alternative is to use the FrameworkElement.FindName() method. In this example, the root element is a DockPanel object. Like all the controls in a WPF window, DockPanel derives from FrameworkElement. That means you can replace the code with this equivalent approach:

```csharp
FrameworkElement frameworkElement = (FrameworkElement)rootElement;
button1 = (Button)frameworkElement.FindName("button1");
```

- **Layout Rounding**

Imagine that a two-column Grid has 200 pixels to work with. If you’ve split it evenly into two proportional columns, that means each gets 100 pixels. But if you have 175 pixels, the division isn’t as clean, and each column gets 87.5 pixels. That means the second column is slightly displaced from the ordinary pixel boundaries. Ordinarily, this isn’t a problem, but if that column contains one of the shape elements, a border, or an image, that content may appear blurry because WPF uses anti-aliasing to “blend” what would otherwise be sharp edges over pixel boundaries.

If this problem affects your layout, there’s an easy fix. Just set the UseLayoutRounding property to true on your layout container. Now WPF will ensure that all the content in that layout container is snapped to the nearest pixel boundary, removing any blurriness.
```xaml
<Grid UseLayoutRounding="True">
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3MjIwODIyM119
-->