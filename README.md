# Nested Grid UI in WinUI DataGrid

## Overview
This repository demonstrates the master-details view pattern in WinUI DataGrid for displaying hierarchical data.

## Master-Details View

The master-details view is a powerful feature that enables hierarchical data visualization in the WinUI DataGrid. It allows you to expand rows to reveal related nested information, creating an intuitive interface for exploring complex datasets.

### Key Concepts

**Master Grid**: The primary DataGrid that displays the parent-level data with expandable rows.

**Details Grid**: A nested DataGrid that appears when a master row is expanded, showing child records related to the selected parent row.

### How It Works

1. **Row Expansion**: Users can click the expand button on any row in the master grid
2. **Data Retrieval**: When expanded, the corresponding detail data is displayed in a nested grid
3. **Multiple Levels**: The details grid can itself contain expandable rows, creating multi-level hierarchies
4. **Data Binding**: Both master and detail grids are bound to data sources that establish parent-child relationships

## Code Samples

### Data Model

The `OrderInfo` class represents the data model with parent and child relationships:

```csharp
public class OrderInfo : NotificationObject, IDisposable
{
    private double _OrderID;
    private string _CustomerID;
    private string _productName;
    private ObservableCollection<OrderInfo> orderDetails;

    public double OrderID
    {
        get { return _OrderID; }
        set
        {
            _OrderID = value;
            RaisePropertyChanged("OrderID");
        }
    }

    public string CustomerID
    {
        get { return _CustomerID; }
        set
        {
            _CustomerID = value;
            RaisePropertyChanged("CustomerID");
        }
    }

    public string ProductName
    {
        get { return _productName; }
        set
        {
            _productName = value;
            RaisePropertyChanged("ProductName");
        }
    }

    public ObservableCollection<OrderInfo> OrderDetails
    {
        get { return this.orderDetails; }
        set
        {
            this.orderDetails = value;
            RaisePropertyChanged("OrderDetails");
        }
    }
}
```

### XAML Definition

Define the master grid with nested details grid:

```xml
<dataGrid:SfDataGrid DataContext="{StaticResource orderInfoViewModel}"
                    x:Name="sfDataGrid"
                    AutoGenerateColumns="False"
                    ItemsSource="{Binding OrdersDetails}"
                    AllowEditing="True">
    
    <!-- Master Grid Columns -->
    <dataGrid:SfDataGrid.Columns>
        <dataGrid:GridNumericColumn HeaderText="Order ID" MappingName="OrderID" TextAlignment="Right"/>
        <dataGrid:GridTextColumn HeaderText="Customer ID" MappingName="CustomerID" />
        <dataGrid:GridDateColumn HeaderText="Order Date" MappingName="OrderDate" TextAlignment="Right" />
        <dataGrid:GridTextColumn HeaderText="Product Name" MappingName="ProductName" />
        <dataGrid:GridNumericColumn HeaderText="Quantity" MappingName="Quantity" />
        <dataGrid:GridNumericColumn HeaderText="Freight" MappingName="Freight" TextAlignment="Right" />
    </dataGrid:SfDataGrid.Columns>

    <!-- Details Grid Definition -->
    <dataGrid:SfDataGrid.DetailsViewDefinition>
        <dataGrid:GridViewDefinition RelationalColumn="OrderDetails">
            <dataGrid:GridViewDefinition.DataGrid>
                <dataGrid:SfDataGrid x:Name="DetailsViewGrid" 
                                    AllowEditing="True"
                                    AutoGenerateColumns="false">
                    <dataGrid:SfDataGrid.Columns>
                        <dataGrid:GridNumericColumn HeaderText="Order ID" MappingName="OrderID" TextAlignment="Right"/>
                        <dataGrid:GridTextColumn HeaderText="Customer ID" MappingName="CustomerID" />
                        <dataGrid:GridNumericColumn HeaderText="Product ID" MappingName="ProductID" TextAlignment="Right" />
                        <dataGrid:GridNumericColumn HeaderText="Quantity" MappingName="Quantity" TextAlignment="Right" />
                        <dataGrid:GridNumericColumn HeaderText="Discount" MappingName="Discount" TextAlignment="Right" />
                        <dataGrid:GridDateColumn HeaderText="Order Date" MappingName="OrderDate" TextAlignment="Right" />
                    </dataGrid:SfDataGrid.Columns>
                </dataGrid:SfDataGrid>
            </dataGrid:GridViewDefinition.DataGrid>
        </dataGrid:GridViewDefinition>
    </dataGrid:SfDataGrid.DetailsViewDefinition>
</dataGrid:SfDataGrid>
```

### ViewModel Setup

The ViewModel populates the master data and binds child collections:

```csharp
public class OrderInfoViewModel : NotificationObject, IDisposable
{
    private ObservableCollection<OrderInfo> _ordersDetails = new ObservableCollection<OrderInfo>();

    public ObservableCollection<OrderInfo> OrdersDetails
    {
        get { return _ordersDetails; }
    }

    public OrderInfoViewModel()
    {
        this.PopulateData();
    }

    private void PopulateData()
    {
        for (int i = 0; i < 100; i++)
        {
            OrderInfo orderInfo = new OrderInfo();
            orderInfo.OrderID = 10000 + i;
            orderInfo.CustomerID = CustomerID[randomValue.Next(0, 14)];
            orderInfo.ProductName = ProductName[randomValue.Next(0, 47)];
            orderInfo.Quantity = randomValue.Next(10, 50);
            orderInfo.Freight = Math.Round(Freight[randomValue.Next(0, 11)], 2);
            
            // Bind child data to OrderDetails collection
            orderInfo.OrderDetails = getorder(orderInfo.OrderID);
            
            _ordersDetails.Add(orderInfo);
        }
    }

    public ObservableCollection<OrderInfo> getorder(double parentOrderID)
    {
        ObservableCollection<OrderInfo> order = new ObservableCollection<OrderInfo>();
        foreach (var childOrder in ord)
            if (childOrder.OrderID == parentOrderID)
                order.Add(childOrder);
        return order;
    }
}
```

## Documentation
For more detailed information about implementing nested grids in WinUI DataGrid, please refer to the official documentation: [Master-Details View - Syncfusion WinUI DataGrid](https://help.syncfusion.com/winui/datagrid/master-details-view)
