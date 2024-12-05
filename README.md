# Pert11-java

|Nama|NIM|Kelas|Mata Kuliah|
|----|---|-----|------|
|**Wasis Wibisono**|**312310661**|**TI.23.A6**|**Pemrograman Orientasi Objek**|

![gambar](Tugas/SS1.png)

# • Buat code java dari diagram class berikut

## • Kelas Payment 
```java
abstract class Payment {
    protected float amount;

    public Payment(float amount) {
        this.amount = amount;
    }

    public float getAmount() {
        return amount;
    }
}
```

### Penjelasan 
```
- Merupakan kelas abstrak dengan atribut amount (jumlah pembayaran).
- Digunakan untuk mendefinisikan struktur pembayaran, tetapi tidak diimplementasikan dalam program ini.
```

## • Kelas Customer
```java
class Customer {
    private final String name;
    private final String address;

    public Customer(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public String getAddress() {
        return address;
    }
}
```

### Penjelasan
```
- Mewakili pelanggan dengan atribut:
  - name: Nama pelanggan.
  - address: Alamat pelanggan.
- Memiliki metode getName() dan getAddress() untuk mengakses informasi pelanggan.
```

## • Kelas Item
```java
class Item {
    private final String shippingWeight;
    private final String description;
    private final float price;
    private final float taxRate;

    public Item(String shippingWeight, String description, float price, float taxRate) {
        this.shippingWeight = shippingWeight;
        this.description = description;
        this.price = price;
        this.taxRate = taxRate;
    }

    public float getPriceForQuantity(int quantity) {
        return price * quantity;
    }

    public float getTax() {
        return price * taxRate;
    }

    public boolean inStock() {
        return true;
    }

    public String getShippingWeight() {
        return shippingWeight;
    }

    public String getDescription() {
        return description;
    }
}
```

### Penjelasan
```
- Mewakili barang yang dipesan dengan atribut:
  - shippingWeight: Berat barang.
  - description: Deskripsi barang.
  - price: Harga barang per unit.
  - taxRate: Pajak barang (dalam persentase).
- Metode penting:
  - getPriceForQuantity(int quantity): Menghitung total harga barang berdasarkan jumlahnya.
  - getTax(): Menghitung pajak barang.
  - getShippingWeight() dan getDescription(): Mengambil berat dan deskripsi barang.
```

## • Kelas OrderDetail
```java
class OrderDetail {
    private final int quantity;
    private final String taxStatus;
    private final Item item;

    public OrderDetail(int quantity, String taxStatus, Item item) {
        this.quantity = quantity;
        this.taxStatus = taxStatus;
        this.item = item;
    }

    public float calcSubTotal() {
        return item.getPriceForQuantity(quantity);
    }

    public float calcWeight() {
        return Float.parseFloat(item.getShippingWeight()) * quantity;
    }

    public float calcTax() {
        return item.getTax();
    }

    public int getQuantity() {
        return quantity;
    }

    public String getTaxStatus() {
        return taxStatus;
    }

    public Item getItem() {
        return item;
    }
}
```

### Penjelasan
```
- Mewakili rincian pesanan untuk setiap barang dengan atribut:
  - quantity: Jumlah barang.
  - taxStatus: Status pajak barang.
  - item: Referensi ke objek Item.
- Metode penting:
  - calcSubTotal(): Menghitung subtotal barang berdasarkan jumlah.
  - calcWeight(): Menghitung total berat barang berdasarkan jumlah.
  - calcTax(): Menghitung pajak untuk barang.
```

## • Kelas Order
```java
class Order {
    private final String date;
    private final String status;
    private final Customer customer;
    private final OrderDetail[] orderDetails;

    public Order(String date, String status, Customer customer, OrderDetail[] orderDetails) {
        this.date = date;
        this.status = status;
        this.customer = customer;
        this.orderDetails = orderDetails;
    }

    public float calcSubTotal() {
        float subTotal = 0;
        for (OrderDetail detail : orderDetails) {
            subTotal += detail.calcSubTotal();
        }
        return subTotal;
    }

    public float calcTax() {
        float tax = 0;
        for (OrderDetail detail : orderDetails) {
            tax += detail.calcTax();
        }
        return tax;
    }

    public float calcTotal() {
        return calcSubTotal() + calcTax();
    }

    public float calcTotalWeight() {
        float totalWeight = 0;
        for (OrderDetail detail : orderDetails) {
            totalWeight += detail.calcWeight();
        }
        return totalWeight;
    }

    public String getDate() {
        return date;
    }

    public String getStatus() {
        return status;
    }

    public Customer getCustomer() {
        return customer;
    }

    public OrderDetail[] getOrderDetails() {
        return orderDetails;
    }
}
```

### Penjelasan
```
- Mewakili pesanan pelanggan dengan atribut:
  - date: Tanggal pesanan.
  - status: Status pesanan.
  - customer: Referensi ke objek Customer.
  - orderDetails: Array dari objek OrderDetail yang berisi rincian barang yang dipesan.
- Metode penting:
  - calcSubTotal(): Menghitung total harga semua barang sebelum pajak.
  - calcTax(): Menghitung total pajak untuk semua barang.
  - calcTotal(): Menghitung total akhir pesanan (subtotal + pajak).
  - calcTotalWeight(): Menghitung berat total semua barang.
```

## • Kelas Main
```java
public class main {
    public static void main(String[] args) {
        Item item1 = new Item("2.5", "Laptop", 200000.0f, 0.05f);
        Item item2 = new Item("0.5", "Mouse", 100000.0f, 0.05f);
        OrderDetail detail1 = new OrderDetail(2, "Taxable", item1);
        OrderDetail detail2 = new OrderDetail(3, "Taxable", item2);

        Customer customer = new Customer("Romi", "123 Main St");

        Order order = new Order("2024-12-05", "Processing", customer, new OrderDetail[]{detail1, detail2});

        System.out.println("Customer: " + order.getCustomer().getName());
        System.out.println("Order Date: " + order.getDate());
        System.out.println("Order Status: " + order.getStatus());

        for (OrderDetail detail : order.getOrderDetails()) {
            Item item = detail.getItem();
            System.out.println("Item: " + item.getDescription());
            System.out.println("Quantity: " + detail.getQuantity());
            System.out.println("Price per item: " + String.format("%.0f", item.getPriceForQuantity(1)));
            System.out.println("SubTotal: " + String.format("%.0f", detail.calcSubTotal())); 
        }

        System.out.println("Order SubTotal: " + String.format("%.0f", order.calcSubTotal())); 
        System.out.println("Order Tax: " + String.format("%.0f", order.calcTax()));
        System.out.println("Order Total: " + String.format("%.0f", order.calcTotal())); 
        System.out.println("Total Weight: " + String.format("%.2f", order.calcTotalWeight()) + " lbs"); 
    }
}

```

### Penjelasan
```
- Membuat Objek Barang (Item):
  - item1 adalah Laptop dengan berat 2.5 lbs, harga 200,000, dan pajak 5%.
  - item2 adalah Mouse dengan berat 0.5 lbs, harga 100,000, dan pajak 5%.
- Membuat Detail Pesanan (OrderDetail):
  - detail1: 2 unit Laptop.
  - detail2: 3 unit Mouse.
- Membuat Pelanggan (Customer):
  - customer: Nama pelanggan Romi dengan alamat 123 Main St.
- Membuat Pesanan (Order):
  - order: Pesanan tanggal 2024-12-05 dengan status Processing, melibatkan 2 detail barang (detail1 dan detail2).
- Mencetak Detail Pesanan:
  - Menampilkan nama pelanggan, tanggal, dan status pesanan.
  - Untuk setiap barang, menampilkan deskripsi, jumlah, harga per unit, dan subtotal.
  - Menampilkan subtotal, pajak, total akhir, dan berat total barang.
```

# • Output
![gambar](Tugas/ss2.png)
