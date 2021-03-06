In certain scenarios it is beneficial to not expose a full collection to consumers of a class. 
Some of these circumstances is when there is additional logic associated with adding/removing items from a collection. 
Because of this reason, it is a good idea to only expose the collection as something you can iterate over without modifying the collection. Let’s take a look at some code

```Java
public class Order {
	private String orderID;
	private String customerName;
	private int value;
	
	public int getValue() {
		return value;
	}
	public void setValue(int value) {
		this.value = value;
	}
	public String getOrderID() {
		return orderID;
	}
	public void setOrderID(String orderID) {
		this.orderID = orderID;
	}
	public String getCustomerName() {
		return customerName;
	}
	public void setCustomerName(String customerName) {
		this.customerName = customerName;
	}
}

public class OrderManager {
	private List<Order> orderList;
	private int ordersTotal =0;
	
	public OrderManager(){
		orderList = new ArrayList<>();
		
	}
	public void addOrder(Order order){
		orderList.add(order);
		ordersTotal += order.getValue();
	}
	public void removeOrder(Order order){
		if(orderList.contains(order)){
			ordersTotal -= order.getValue();
			orderList.remove(order);
		}
	}
	public int getOrdersTotal(){
		return ordersTotal;
	}
}

```

As you can see, we have encapsulated the collection as to not expose the Add/Remove methods to consumers of this class. There is some other types in the .Net framework that will produce different behavior for encapsulating a collection such as ReadOnlyCollection but they do have different caveats with each. This is a very straightforward refactoring and one worth noting. Using this can ensure that consumers do not mis-use your collection and introduce bugs into the code.

#### Original C# code from the book:
``` 
   1: public class Order
   2: {
   3:     private List<OrderLine> _orderLines;
   4:  
   5:     public IEnumerable<OrderLine> OrderLines
   6:     {
   7:         get { return _orderLines; }
   8:     }
   9:  
  10:     public void AddOrderLine(OrderLine orderLine)
  11:     {
  12:         _orderTotal += orderLine.Total;
  13:         _orderLines.Add(orderLine);
  14:     }
  15:  
  16:     public void RemoveOrderLine(OrderLine orderLine)
  17:     {
  18:         orderLine = _orderLines.Find(o => o == orderLine);
  19:         if (orderLine == null) return;
  20:  
  21:         _orderTotal -= orderLine.Total
  22:         _orderLines.Remove(orderLine);
  23:     }
  24: }
 ```