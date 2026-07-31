<div align="center">

# Flipkart E-Commerce Database Design

### DBMS Assignment: Requirements, ER Diagram and Relational Schema

**Prepared by:** Santhosh  
**Register Number:** AADS25023  
**Programme:** B.Sc. Artificial Intelligence and Data Science - II Year  
**Institution:** AMET University

</div>

---

## Project Overview

This project presents a basic database design for a Flipkart-style e-commerce
platform. It identifies the main entities, their attributes, primary keys,
foreign keys and relationships. The Entity Relationship Diagram was designed
using StarUML and converted into a relational schema.

This is a simplified academic model. It does not represent Flipkart's actual
internal database.

## Assignment Tasks

### Task I: Software Requirements Specification

The functional and non-functional requirements of the e-commerce platform were
identified. The system supports customer accounts, sellers, products, carts,
wishlists, orders, payments, delivery tracking and product reviews.

### Task II: Database Requirements

The required entities, attributes, primary keys, foreign keys and relationships
were identified.

### Task III: ER Diagram and Relational Schema

The ER diagram was created in StarUML using Crow's Foot notation. The completed
diagram was then converted into a relational schema.

## ER Diagram

<p align="center">
  <img src="https://github.com/santhosh-v07/DBMS-TASKS/blob/main/Task_3/Flipkart%20E-Commerce%20ER%20Diagram.jpg?raw=true"
       alt="Flipkart E-Commerce ER Diagram"
       width="95%">
</p>

<p align="center">
  <em>Figure 1: ER Diagram of Flipkart E-Commerce Platform</em>
</p>

## Main Entities

| Entity | Primary Key | Purpose |
|:---|:---:|:---|
| Customer | `customer_id` | Stores customer account information |
| Address | `address_id` | Stores customer delivery addresses |
| Seller | `seller_id` | Stores seller and business information |
| Category | `category_id` | Groups similar products |
| Product | `product_id` | Stores general product information |
| Product Listing | `listing_id` | Connects products with sellers, prices and stock |
| Cart | `cart_id` | Stores the shopping cart owned by a customer |
| Cart Item | `cart_id + listing_id` | Stores product listings added to a cart |
| Wishlist | `wishlist_id` | Stores a customer's wishlist |
| Wishlist Item | `wishlist_id + product_id` | Stores products saved in a wishlist |
| Customer Order | `order_id` | Stores orders placed by customers |
| Order Item | `order_item_id` | Stores the individual items in an order |
| Payment | `payment_id` | Stores payment attempts and results |
| Delivery | `delivery_id` | Stores shipment and tracking information |
| Review | `review_id` | Stores product ratings and comments |

## Main Relationships

| Parent Entity | Cardinality | Related Entity |
|:---|:---:|:---|
| Customer | 1:M | Address |
| Customer | 1:1 | Cart |
| Cart | 1:M | Cart Item |
| Product Listing | 1:M | Cart Item |
| Customer | 1:1 | Wishlist |
| Wishlist | 1:M | Wishlist Item |
| Product | 1:M | Wishlist Item |
| Category | 1:M | Product |
| Product | 1:M | Product Listing |
| Seller | 1:M | Product Listing |
| Customer | 1:M | Customer Order |
| Address | 1:M | Customer Order |
| Customer Order | 1:M | Order Item |
| Product Listing | 1:M | Order Item |
| Customer Order | 1:M | Payment |
| Customer Order | 1:1 | Delivery |
| Customer | 1:M | Review |
| Product | 1:M | Review |

## Relational Schema

```text
CUSTOMER(
  customer_id PK,
  name,
  email,
  password_hash,
  phone
)

ADDRESS(
  address_id PK,
  customer_id FK,
  address,
  city,
  state,
  postal_code
)

SELLER(
  seller_id PK,
  business_name,
  email,
  gst_number,
  status
)

CATEGORY(
  category_id PK,
  category_name
)

PRODUCT(
  product_id PK,
  category_id FK,
  name,
  brand,
  description
)

PRODUCT_LISTING(
  listing_id PK,
  product_id FK,
  seller_id FK,
  price,
  stock
)

CART(
  cart_id PK,
  customer_id FK,
  created_at
)

CART_ITEM(
  cart_id PK/FK,
  listing_id PK/FK,
  quantity,
  added_at
)

WISHLIST(
  wishlist_id PK,
  customer_id FK,
  created_at
)

WISHLIST_ITEM(
  wishlist_id PK/FK,
  product_id PK/FK,
  added_at
)

CUSTOMER_ORDER(
  order_id PK,
  customer_id FK,
  address_id FK,
  order_date,
  status,
  total
)

ORDER_ITEM(
  order_item_id PK,
  order_id FK,
  listing_id FK,
  quantity,
  price
)

PAYMENT(
  payment_id PK,
  order_id FK,
  method,
  amount,
  status
)

DELIVERY(
  delivery_id PK,
  order_id FK,
  tracking_number,
  delivery_status
)

REVIEW(
  review_id PK,
  customer_id FK,
  product_id FK,
  rating,
  comment
)
```

## Identifying Relationships

Only the following relationships are identifying because the foreign keys are
also part of the child entity's primary key:

- `CART → CART_ITEM`
- `PRODUCT_LISTING → CART_ITEM`
- `WISHLIST → WISHLIST_ITEM`
- `PRODUCT → WISHLIST_ITEM`

All other relationships are non-identifying.

## Tools Used

| Tool | Purpose |
|:---|:---|
| StarUML | ER diagram design |
| Microsoft Word | Assignment documentation |
| GitHub | Repository and file submission |

## Suggested Repository Structure

```text
flipkart-database-design/
├── README.md
├── assets/
│   └── flipkart-er-diagram.png
├── docs/
│   ├── Task-I-SRS.pdf
│   ├── Task-II-Database-Requirements.docx
│   └── Task-III-ER-Diagram-and-Schema.pdf
└── staruml/
    └── Flipkart-ER-Diagram.mdj
```

## Adding the ER Diagram Image

1. Open the completed diagram in StarUML.
2. Select **File → Export Diagram As → PNG**.
3. Create an `assets` folder in the GitHub repository.
4. Rename the exported image to `flipkart-er-diagram.png`.
5. Upload it to the `assets` folder.
6. The image will automatically appear in this README.

## Conclusion

The database design covers the basic operations of an e-commerce platform. The
ER diagram clearly shows the entities and relationships, while the relational
schema shows how the data can be stored as connected database tables.

---

<p align="center">
  <strong>DBMS Assignment | AMET University | Santhosh</strong>
</p>
