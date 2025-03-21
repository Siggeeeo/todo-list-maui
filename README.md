MainPage.xaml

<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TodoApp.MainPage"
             Title="TodoApp">

    <VerticalStackLayout Padding="20" Spacing="15">
        <Label 
            Text="Min TODO-lista"
            FontSize="24"
            HorizontalOptions="Center"
            Margin="0,0,0,20" />

        <Grid ColumnDefinitions="*,Auto" RowSpacing="10">
            <Entry x:Name="AddListItem" 
                   Placeholder="Lägg till uppgift" 
                   Grid.Column="0" />

            <Button Text="Lägg till" 
                    Clicked="OnAddClicked" 
                    Grid.Column="1" 
                    Margin="5,0,0,0" />
        </Grid>

        <CollectionView ItemsSource="{Binding Items}" Margin="0,10,0,0">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <SwipeView>
                        <SwipeView.RightItems>
                            <SwipeItems Mode="Execute">
                                <SwipeItem Text="Ta bort" 
                                           BackgroundColor="Red" 
                                           Invoked="OnDeleteSwipe" />
                            </SwipeItems>
                        </SwipeView.RightItems>
                        <Frame Margin="0,5" Padding="10" BorderColor="LightGray">
                            <Label Text="{Binding .}" 
                                   FontSize="18" 
                                   VerticalOptions="Center" />
                        </Frame>
                    </SwipeView>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </VerticalStackLayout>
</ContentPage>

_______________________________________________________________________________________

using System.Collections.ObjectModel;

namespace TodoApp;

public partial class MainPage : ContentPage
{
    public ObservableCollection<string> Items { get; set; } = new();

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }

    private void OnAddClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(AddListItem.Text))
        {
            Items.Add(AddListItem.Text);
            AddListItem.Text = "";
        }
    }

    private void OnDeleteSwipe(object sender, EventArgs e)
    {
        var item = (sender as SwipeItem)?.BindingContext as string;
        if (item != null && Items.Contains(item))
        {
            Items.Remove(item);
        }
    }
}
