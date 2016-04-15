
# WPF #

### XAML ###
#### Special Characters ####
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

#### Using Types from Other Namespaces ####
**Naming Conventions**
- *xmlns:Prefix="clr-namespace:Namespace;assembly=AssemblyName"*
- *xmlns:sys="clr-namespace:System;assembly=mscorlib"*
- *xmlns:local="clr-namespace:MyNamespace"*