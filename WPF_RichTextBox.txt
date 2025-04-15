
# WPF RichTextBox Alkalmazás - Magyar Magyarázó Fájl

Ez a WPF alkalmazás egy egyszerű szövegszerkesztőt valósít meg, amely különböző szöveges formázási lehetőségeket kínál a felhasználóknak. A programban a felhasználók a **Félkövér**, **Dőlt**, **Aláhúzott** formázásokat alkalmazhatják, valamint módosíthatják a betűméretet, a betűszínt, a háttérszínt és a betűtípust. Ezen kívül a dokumentumot el lehet menteni és vissza lehet tölteni.

## Felhasználói Felület

A felhasználói felület a következő főbb elemekből áll:

- **Eszköztár (ToolBar):** Az eszköztár a szövegszerkesztéshez szükséges formázási lehetőségeket tartalmazza, mint például a félkövér, dőlt, aláhúzott szöveg, betűméret növelés/csökkentés, valamint a szöveg igazítása.
- **RichTextBox:** A szövegbevitelhez és -formázáshoz használt vezérlőelem.
- **Színválasztók:** A szöveg színének és a háttérszínének módosítására szolgáló legördülő menük.
- **Betűtípus választó:** A különböző betűtípusok kiválasztására szolgáló legördülő menü.
- **Mentés és Betöltés gombok:** A szöveg mentésére és betöltésére szolgáló gombok.

## XAML Kód

A felhasználói felületet XAML-ban definiáljuk:

```xml
<Window x:Class="Wpf_1_RichTextBox.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <DockPanel Name="mainPanel">
            <ToolBar Name="mainToolBar" Height="30" DockPanel.Dock="Top">
                <!-- Gombok és szöveges elemek -->
            </ToolBar>
            <ToolBar DockPanel.Dock="Bottom">
                <Button Click="SaveRTBContent">Save</Button>
                <Button Click="LoadRTBContent">Load</Button>
            </ToolBar>
            <RichTextBox Name="mainRTB" AcceptsTab="True"/>
        </DockPanel>
    </Grid>
</Window>
```

## C# Kód

A **MainWindow.xaml.cs** fájl tartalmazza a szövegszerkesztő logikáját:

### Mentés Funkció

A szöveg mentésére a `SaveRTBContent` metódust használjuk. Ez a metódus a szöveges tartalmat XAML csomag formátumban menti a fájlba.

```csharp
private void SaveRTBContent(object sender, RoutedEventArgs e)
{
    string path = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "test.xaml");
    TextRange range = new TextRange(mainRTB.Document.ContentStart, mainRTB.Document.ContentEnd);

    using (FileStream fStream = new FileStream(path, FileMode.Create))
    {
        range.Save(fStream, DataFormats.XamlPackage);
    }

    MessageBox.Show("Mentés sikeres: " + path);
}
```

### Betöltés Funkció

A betöltéshez hasonlóan a `LoadRTBContent` metódust használjuk. Ha a fájl létezik, a tartalom betöltésre kerül.

```csharp
private void LoadRTBContent(object sender, RoutedEventArgs e)
{
    string path = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "test.xaml");

    if (File.Exists(path))
    {
        TextRange range = new TextRange(mainRTB.Document.ContentStart, mainRTB.Document.ContentEnd);
        using (FileStream fStream = new FileStream(path, FileMode.Open))
        {
            range.Load(fStream, DataFormats.XamlPackage);
        }
        MessageBox.Show("Betöltés sikeres: " + path);
    }
    else
    {
        MessageBox.Show("A fájl nem található: " + path);
    }
}
```

### Betűméret Frissítése

A `NumberRefreshButton_Click` metódus frissíti a betűméret kijelzőt, amikor a felhasználó módosítja a betűméretet.

```csharp
private void NumberRefreshButton_Click(object sender, RoutedEventArgs e)
{
    if (sender is Button button)
    {
        var meret = mainRTB.Selection.GetPropertyValue(TextElement.FontSizeProperty);

        if (meret != DependencyProperty.UnsetValue)
            FontSizeIndicator.Text = meret.ToString();
        else
            FontSizeIndicator.Text = "-";
    }
}
```

### Betűszín Választás

A `FontColorComboBox_SelectionChanged` metódus frissíti a szöveg színét a felhasználó által választott színnek megfelelően.

```csharp
private void FontColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (FontColorComboBox.SelectedItem is ComboBoxItem selectedItem)
    {
        string color = selectedItem.Tag.ToString();
        mainRTB.Foreground = new SolidColorBrush((Color)ColorConverter.ConvertFromString(color));
    }
}
```

### Háttérszín Választás

A `BackgroundColorComboBox_SelectionChanged` metódus frissíti a szöveg háttérszínét a felhasználó által választott színnek megfelelően.

```csharp
private void BackgroundColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (BackgroundColorComboBox.SelectedItem is ComboBoxItem selectedItem)
    {
        string color = selectedItem.Tag.ToString();
        mainRTB.Background = new SolidColorBrush((Color)ColorConverter.ConvertFromString(color)); 
    }
}
```

### Betűtípus Választás

A `FontFamilyComboBox_SelectionChanged` metódus frissíti a szöveg betűtípusát a felhasználó által választott betűtípusnak megfelelően.

```csharp
private void FontFamilyComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (FontFamilyComboBox.SelectedItem is ComboBoxItem selectedItem)
    {
        string fontFamily = selectedItem.Tag.ToString();
        mainRTB.FontFamily = new FontFamily(fontFamily);
    }
}
```

## Következtetés

Ez a WPF alkalmazás egy egyszerű szövegszerkesztőt kínál a felhasználóknak, amely lehetővé teszi számukra, hogy különböző formázásokat alkalmazzanak a szövegre, valamint elmentsék és betöltsék a dokumentumokat XAML formátumban. Az alkalmazás egyszerűen bővíthető további funkciókkal, például képek, táblázatok vagy egyéb formázási lehetőségek hozzáadásával.
