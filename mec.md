# CHANGELOG -> 3 WEEKS AGO
### CHANGES MADE TO BUSINESS
- Created a service file in service directory and moved business logic from `business.py` route to a `busines.py` file created in the service directory.

- *Files modified for business:*
- `business.py` -> `mercury/routes`
- `business.py` -> `mercury/services`

### CHANGES MADE TO SCHEMA -> *mercury/core/schemas*
-*Schemas added:*

- `AggregateTxSchema`  
- `AggregatesSchemaTx`

### CHANGES MADE TO INVENTORY -> *mercury/routes/inventory*
- New function `get_aggregates` was created.

# CHANGELOG -> 2 WEEKS AGO 
### CHANGES MADE TO INVENTORY
- Created a service file in service directory and moved inventory logic from `inventory.py` route to a `inventory.py` file created in the service directory.

- *Files modified for inventory:*
- `inventory.py` -> `mercury/routes`
- `inventory.py` -> `mercury/services`

### CHANGES MADE TO INVOICE
- Created a service file in service directory and moved invoice logic from `invoice.py` route to a `invoice.py` file created in the service directory. Added unit test for invoice as well.

- *Files modified for invoice:*
- `invoice.py` -> `mercury/routes`
- `invoice.py` -> `mercury/services`
- `test_invoice.py` ->`test/test_invoice`

### CHANGES MADE TO ORDER
- Created a service file in service directory and moved order logic from `order.py` route to a `order.py` file created in the service directory. Added unit test for invoice as well.

- *Files modified for order:*
- `order.py` -> `mercury/routes`
- `order.py` -> `mercury/services`


# CHANGELOG -> RECENT (< 1 WEEK)
### CHANGES MADE TO ORDER
- Added a new route `export_order` in `mercury/routes/order` to export orders in csv and xlsx formats.
- Added the logic for the new route `export_order` in `mercury/service/order`.
- Created a new utils file for exporting orders in `mercury/utils/order_utils`


### CHANGES MADE TO MODELS -> *mercury/core/model*
  - Added a new field `qrcode: Optional[str]` to the `InventoryItem` model to store the QR code data.

### CHANGES MADE TO INVENTORY -> *mercury/services/inventory*
- *QR Code Generation:*
- Integrated generate_qr_code utility to create QR codes for inventory items.
- QR code is stored in the qrcode field of the InventoryItem model.
- *Validation for Categories:*
- Ensures the category (category_id) exists before creating or updating inventory items.

### CHANGES MADE TO UTILS -> *mercury/utilities/qrcode_utils*
- *New File:*
  - Introduced a utility function `generate_qr_code`
    - Generates a QR code for a given item ID.
    - Returns the QR code as a Base64-encoded string.

### CHANGES MADE TO TEST_INVENTORY -> *tests/test_inventory*
- *New Tests:*
  - Added tests for:
    - Creating an inventory item with a QR code.
    - Validating the QR code content and ensuring it matches the item ID.
    - Handling cases where the category does not exist while creating an inventory item.
    - Enhanced test coverage for QR code generation and barcode scanning.