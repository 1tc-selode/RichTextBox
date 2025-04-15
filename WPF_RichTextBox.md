
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
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Wpf_1_RichTextBox"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <DockPanel Name="mainPanel">
            <ToolBar Name="mainToolBar" Height="30" DockPanel.Dock="Top">
                <Button  Command="EditingCommands.ToggleBold" ToolTip="Félkövér">
                    <TextBlock FontSize="14" Width="20" FontWeight="Bold">B</TextBlock>
                </Button>
                <Button Command="EditingCommands.ToggleItalic" ToolTip="Dőlt">
                    <TextBlock FontSize="14" Width="20" FontStyle="Italic" FontWeight="Bold">I</TextBlock>
                </Button>
                <Button Command="EditingCommands.ToggleUnderline" ToolTip="Aláhúzott">
                    <TextBlock FontSize="14" Width="20" TextDecorations="Underline" FontWeight="Bold">U</TextBlock>
                </Button>
                <Button Command="EditingCommands.IncreaseFontSize" Click="NumberRefreshButton_Click" ToolTip="Betűméret növelése">
                    <TextBlock FontSize="14" Width="20" FontWeight="Bold">A+</TextBlock>
                </Button>
                <Button Command="EditingCommands.DecreaseFontSize" Click="NumberRefreshButton_Click" ToolTip="Betűméret csökkentése">
                    <TextBlock FontSize="14" Width="20" FontWeight="Bold" >A-</TextBlock>
                </Button>
                <TextBlock Name="FontSizeIndicator" Text="12" FontSize="14" VerticalAlignment="Center" FontWeight="Bold" Margin="0,0,10,0"/>

                <Button Command="EditingCommands.ToggleBullets" ToolTip="Pontozás">
                    <TextBlock FontSize="14" Width="35" FontWeight="Bold">pont</TextBlock>
                </Button>
                <Button Command="EditingCommands.ToggleNumbering" ToolTip="Számozás">
                    <TextBlock FontSize="14" Width="40" FontWeight="Bold">szám</TextBlock>
                </Button>
                <Button Command="EditingCommands.AlignLeft" ToolTip="Balra rendezés">
                    <TextBlock FontSize="14"  Width="30" FontWeight="Bold">bal</TextBlock>
                </Button>
                <Button Command="EditingCommands.AlignCenter" ToolTip="Középre rendezés">
                    <TextBlock FontSize="14" Width="45" FontWeight="Bold">közép</TextBlock>
                </Button>
                <Button Command="EditingCommands.AlignRight" ToolTip="Jobbra rendezés">
                    <TextBlock FontSize="14" Width="35" FontWeight="Bold">jobb</TextBlock>
                </Button>
                <Button Command="EditingCommands.AlignJustify" ToolTip="Sorkizárás">
                    <TextBlock FontSize="14" Width="55" FontWeight="Bold">sorkizár</TextBlock>
                </Button>
                <TextBlock Text="Betű színe: " VerticalAlignment="Center" FontWeight="Bold" FontSize="14"></TextBlock>
                <ComboBox Name="FontColorComboBox" SelectionChanged="FontColorComboBox_SelectionChanged" Height="30">
                    <ComboBoxItem Content="Piros" Tag="Red"/>
                    <ComboBoxItem Content="Zöld" Tag="Green"/>
                    <ComboBoxItem Content="Kék" Tag="Blue"/>
                    <ComboBoxItem Content="Fekete" Tag="Black"/>
                    <ComboBoxItem Content="Sárga" Tag="Yellow"/>
                </ComboBox>
                <TextBlock Text="Háttér színe: " VerticalAlignment="Center" FontWeight="Bold" FontSize="14"></TextBlock>
                <ComboBox Name="BackgroundColorComboBox" SelectionChanged="BackgroundColorComboBox_SelectionChanged">
                    <ComboBoxItem Content="Fehér"  Tag="White"/>
                    <ComboBoxItem Content="Kék" Tag="LightBlue"/>
                    <ComboBoxItem Content="Zöld" Tag="LightGreen"/>
                    <ComboBoxItem Content="Sárga" Tag="LightYellow"/>
                    <ComboBoxItem Content="Szürke" Tag="LightGray"/>
                </ComboBox>
                <TextBlock Text="Betűtípus: " VerticalAlignment="Center" FontWeight="Bold" FontSize="14"></TextBlock>
                <ComboBox Name="FontFamilyComboBox" SelectionChanged="FontFamilyComboBox_SelectionChanged">
                    <ComboBoxItem Content="Arial" Tag="Arial"/>
                    <ComboBoxItem Content="Times New Roman" Tag="Times New Roman"/>
                    <ComboBoxItem Content="Courier New" Tag="Courier New"/>
                </ComboBox>
            </ToolBar>
            <ToolBar DockPanel.Dock="Bottom">
                <Button Click="SaveRTBContent" FontWeight="Bold" Margin="10,0,20,0">Save</Button>
                <Button Click="LoadRTBContent" FontWeight="Bold">Load</Button>
            </ToolBar>

            <RichTextBox Name="mainRTB" AcceptsTab="True"/>
            
        </DockPanel>
    </Grid>
</Window>

```

## C# Kód

A **MainWindow.xaml.cs** fájl tartalmazza a szövegszerkesztő logikáját:

### Betűméret Frissítése

A `NumberRefreshButton_Click` metódus frissíti a betűméret kijelzőt, amikor a felhasználó módosítja a betűméretet.
- A mainRTB.Selection az aktuálisan kijelölt szövegrészt jelenti a RichTextBox-ban.
- A GetPropertyValue(...) függvény lekéri egy adott tulajdonság (itt a FontSize, azaz betűméret) konkrét értékét a kijelölt szövegre vonatkozóan.
- Ha a kijelölt rész többféle betűméretet tartalmaz, akkor visszaad egy speciális értéket (DependencyProperty.UnsetValue), különben visszaadja a tényleges méretet (pl. 12.0).
```csharp
private void NumberRefreshButton_Click(object sender, RoutedEventArgs e)
{
    if (sender is Button button)
    {
        object meret = mainRTB.Selection.GetPropertyValue(TextElement.FontSizeProperty);
        FontSizeIndicator.Text = meret.ToString();
    }
}
```

### Betűszín Választás

A `FontColorComboBox_SelectionChanged` metódus frissíti a szöveg színét a felhasználó által választott színnek megfelelően.
- A kiválasztott színnevet átalakítja egy Color objektummá (ColorConverter.ConvertFromString(color)).
- Ebből létrehoz egy SolidColorBrush-t (egyszínű ecsetet), és
- Beállítja ezt szövegszínként (Foreground) a RichTextBox-ra.
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
- A color string értéket átalakítja egy Color típusú objektummá a ColorConverter.ConvertFromString() segítségével.
- Ezután létrehoz belőle egy SolidColorBrush-t (egyszínű festékecset).
- Végül ezt az ecsetet beállítja a mainRTB háttérszíneként (Background tulajdonság).
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
- A szövegként kapott betűtípusnévből létrehoz egy FontFamily objektumot.
- Ezután ezt a betűtípus-családot beállítja a RichTextBox (mainRTB) FontFamily tulajdonságának értékeként.
- Ennek hatására a teljes szöveg megjelenése megváltozik az új betűtípusnak megfelelően.
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

### Mentés Funkció

A szöveg mentésére a `SaveRTBContent` metódust használjuk. Ez a metódus a szöveges tartalmat XAML csomag formátumban menti a fájlba.
1. - Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments) → Ez lekéri a felhasználó gépén a "Dokumentumok" mappa elérési útját (pl. C:\Users\Petike\Documents).
   - Path.Combine(...) → Összefűzi ezt az elérési utat a "test.xaml" fájlnévvel. Így kapsz egy teljes fájlútvonalat, ahova a fájl mentésre kerül.
2. - TextRange egy osztály, amely egy szövegrészt jelöl ki WPF-ben.
   - Itt a mainRTB.Document.ContentStart és ContentEnd segítségével a teljes RichTextBox tartomány kerül kijelölésre.
3. - FileStream megnyit egy új fájlt írásra az előzőleg megadott útvonalon.
           - FileMode.Create: ha a fájl már létezik, felülírja azt, ha nem létezik, létrehozza.
   - range.Save(...) → Elmenti a kiválasztott szövegrészt (range) az adott fájlba a megadott formátumban. → A formátum itt: DataFormats.XamlPackage, ami lehetővé teszi a szöveg formázott (pl. félkövér, színes, aláhúzott stb.) mentését, nem csak egyszerű szövegként.
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
