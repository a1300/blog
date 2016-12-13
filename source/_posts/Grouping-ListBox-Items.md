---
title: Grouping ListBox-Items
date: 2016-12-13 01:21:35
tags: WPF, ListBox, CollectionViewSource
---

Imagine you want to give the user the option to specify the columns of an DataGrid or GridView. You could pop out a dialog that shows all available columns on the left side and all visible columns on the right side. Like in the following picture.

{% asset_img before.png Simple Options %}

This example shows two ListBoxes that bind to two collections in a {% link MVVM https://msdn.microsoft.com/en-us/magazine/dd419663.aspx "Model-View-ViewModel Design Pattern" %}-Manner. 
The underlying model is a simple class.

```
public class Category
{
    public string Name { get; set; }
    public CategoryType Type { get; set; }

    public override string ToString()
    {
        return Name ?? "No Category-Name";
    }
}
public enum CategoryType
{
    Image = 0,
    GeneralInformation = 1,
    AddressData
}
```

------------


If you want to group belonging options together you could make use of the ''CollectionViewSource''. That way the user's eye must not search the whole list to find belonging items.


{% asset_img after.png This is an example image %}

In the constructor of the ViewModel will be the ObservableCollection of Category initialized and populated with Dummy-Data.
```
AvailableColumns = new ObservableCollection<Category>();
AvailableColumns.Add(new Category() 
    { Name = "Thumbnail", Type = CategoryType.Image });
AvailableColumns.Add(new Category() 
    { Name = "LastName", Type = CategoryType.GeneralInformation });
AvailableColumns.Add(new Category() 
    { Name = "ZIP", Type = CategoryType.AddressData });
```

In the view binds the 'CollectionViewSource' to the ObservableCollection AvailableColumns. Furthermore is the CollectionViewSource.GroupDescriptions set. The PropertyName is set to "Type" of the Class Category. Category is the underlying Model for the Collection.
```
<CollectionViewSource x:Key="GroupedItemsLeft" Source="{Binding AvailableColumns}">
    <CollectionViewSource.GroupDescriptions>
        <PropertyGroupDescription PropertyName="Type"/>
    </CollectionViewSource.GroupDescriptions>
</CollectionViewSource>
```

{% asset_img PropertyName_Type.PNG Show the PropertyName "Type" which uses the CollectionViewSource as GroupDescription %}


The CollectionViewSource with the Key "GroupedItemsLeft" can now be used as Binding-Source for the ListBox. Please note that the CollectionViewSource doesn't changes the data of the underlying ObservableCollection. It can display the data for the user in a different way.
```
<ListBox ItemsSource="{Binding Source={StaticResource GroupedItemsLeft}, 
    Mode=OneWay, UpdateSourceTrigger=PropertyChanged}"/>
```


Entire Source-Code on {% link GitHub https://github.com/a1300/TonyW_Expanders_Example a1300 %}-Repo.

{% blockquote "Inspiration from SO-User Michael Stoll" http://stackoverflow.com/questions/2595669/bind-listbox-in-wpf-with-grouping/2598299#2598299 CollectionViewSource %}
{% endblockquote %}