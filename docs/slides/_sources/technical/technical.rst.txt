945 Technical Manual
====================

945 Development Concept
-----------------------
* ออกแบบระบบโดยเน้นการคงการทำงานด้านบัญชีตามมาตรฐาน Odoo ให้มากที่สุดโดยอิงหลักการ :ref:`Odoo + OCA + Custom<odoo_oca_custom>`
* ส่วนโมดูลเพิ่มเติม เน้นการใช้งาน OCA modules ก่อนหลังจากนั้นจึงใช้ Customer Codes ดูคำบรรยายของแต่ละโมดูลเสริมได้จาก :ref:`Modulesx<modules>`
* การยิง API มีการยิงเข้า 2 จุดหลัก คือเข้าที่หน้าต่าง Sales Order และเข้าที่หน้าต่าง 945 Operations โดยการยิง Reference API สามารถดูได้ที่ :ref:`APIs<apis>`
* การยิง API เข้ามาในระบบจะเป็นการยิงเข้า :ref:`Job Queue<job_queue>` เสมอเพื่อให้ระบบมีความเร็วและรับโหลดได้มาก

.. nextslide::

* กรณียิง API เข้า Sales Order ในบางประเภทจะมีการ Compute Action เพื่อทำการปรับปรุงรายการให้เข้ากับแบบแผนการคำนวนของ 945
  และใช้ :ref:`Sale Automatic Workflow<sale_automatic_workflow>` เพื่อทำการวิ่งเอกสารต่อไปยัง Account Invoice เพื่อทำการบันทึกบัญชี (ตามหลัก Odoo ปกติ)
* กรณียิง API เข้า 945 Operations (ซึ่งมีหลาย windows/model ในการรับข้อมูล) จะมีการสร้าง Journal ด้วย :ref:`Account Move Template<account_move_template>`
* ในกรณีมีการคำนวนที่่มีความซับซ้อน ระบบจะเรียกใช้งาน :ref:`Compute Helper<compute_helper>`
* มี :ref:`Unit Test<unit_test>` สำหรับ process ที่สำคัญ

.. _odoo_oca_custom:

Odoo + OCA + 945
----------------
1. Odoo Core
2. Odoo Enterprise version 13
3. **Odoo Community Association (OCA)**
4. **Customer Codes (945)**

.. image:: images/eco.png
    :align: center

.. _modules:

> Modules
==========

OCA Modules
-----------

* l10n_th_xxx (โมดูลที่เริ่มต้นด้วย)
    เกี่ยวกับภาษีไทย - WHT, VAT, Undue VAT, Tax Reports.
* queue_job
    Base module สำหรับ Queue Job ถูกเรียกใช้โดย sunteen_api ในการเปลี่ยน function ให้เป็นแบบ queue :code:`@job_auto_delay`
* sale_automatic_workflow, sale_automatic_workflow_job
    โมดูลเสริมเพื่อให้ workflow จาก Sales Order -> Invoice เกิดขึ้นโดยอัตโนมัติ
* account_mass_reconcile
    โมดูลเสริมเพื่อให้ผู้ใช้งานทางบัญชีสามารถจับคู่เคลียร์ได้่ง่ายขึ้น โดยตั้งค่าการเคลียร์แบบอัตโนมัติได้

.. nextslide::

* account_move_template, account_move_template_enhanced
    ช่วยในการสร้าง Template สำหรับการบันทึกบัญชี เรียกใช้โดย Operations ต่างๆ
* account_payment_term_extension
    เพิ่ม Payment Term ตามโจทย์ของ 945 เช่นเอกสารในอาทิตย์นี้ทั้งหมดให้ไปกองวันครบกำหนดที่ศูกร์หน้า เป็นต้น

.. nextslide::

945 Modules
-----------

* sunteen
    โมดูลหลักสำหรับ Business Process ของ 945 ทั้งหมด เช่น eCommerce, Express.
* sunteen_install
    ทำหน้าที่่เรียกการติดตั้งโมดูลทั่งหมดที่ต้องใช้ใน 945 โดยตัวเองไม่ได้มี logic ใดๆ
* sunteen_coa, sunteen_data, sunteen_form
    ทำหน้าที่โหลดข้อมูลที่ต้องใช้งาน เช่น Chart of Account, Move Templates, การตั้งค่าต่างๆ และแบบฟอร์ม
* sunteen_mass_reconcile
    เสริมการทำงานของ account_mass_reconcile เพื่อให้จับคู่เคลียร์ผ่าน Parcel Number ได้

.. nextslide::

* sunteen_test
    โมดูลเสริมสำหรับการทำ Unit Test

.. _apis:

> 945 APIs
==========

ตัวอย่างการใช้งาน APIs ระบบ 945

eCommerce
---------

* eCommerce - Standard
* eCommerce - Consignment Fix GP
* eCommerce - Consignment VAR GP

eCommerce - Standard
####################

API 1 (sale.order)

.. code-block:: python

    {
        "payload": {
            "status": "out of delivery",  # Status: out of delivery
            "partner_id": 1,  # Customer (res.partner)
            "sunteen_dealer_id": 2,  # Dealer (res.partner)
            "workflow_process_id": "eCommerce Standard",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "sunteen_payment_method_id": 3,  # Payment Method (sunteen.payment.method)
            "sunteen_payment_provider_id": 34,  # Payment Provider (res.partner)
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                },
                {
                    "sunteen_parcel_number": "TDZ001",
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                },
                {
                    "sunteen_parcel_number": "TDZ001",  # Dealer Amount Difference Line
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 18,
                    "product_uom_qty": 1,
                    "dealer_price_unit": 20.0,  # API sent +, odoo change to -
                },
            ]
        },
        "auto_create": {    # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }

API2 (sunteen.delivery.complete):


.. code-block:: python

    {
        "payload": {
            "status": "completed",  # Status : completed/return
            "partner_id": 1,  # Customer (res.partner)
            "sunteen_dealer_id": 2,  # Dealer (res.partner)
            "workflow_process_id": "eCommerce Standard",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "sunteen_payment_method_id": 3,  # Payment Method (sunteen.payment.method)
            "sunteen_payment_provider_id": 34,  # Payment Provider (res.partner)
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                },
                {
                    "sunteen_parcel_number": "TDZ001",
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                },
                {
                    "sunteen_parcel_number": "TDZ001",  # Dealer Amount Difference Line
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 18,
                    "product_uom_qty": 1,
                    "dealer_price_unit": 20.0,  # API sent +, odoo change to -
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }


eCommerce - Consignment Fix GP
##############################

API1 (sunteen.record.transportation.cost):

.. code-block:: python

    {
        "payload": {
            "status": "out of delivery",  # Status: out of delivery
            "partner_id": 1,  # Customer=Consignor (res.partner)
            "sunteen_dealer_id": 2,  # Dealer (res.partner)
            "workflow_process_id": "eCommerce Consignment Fix GP",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "sunteen_payment_method_id": 3,  # Payment Method (sunteen.payment.method)
            "sunteen_payment_provider_id": 34,  # Payment Provider (res.partner)
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                },
                {
                    "sunteen_parcel_number": "TDZ001",
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                },
                {
                    "sunteen_parcel_number": "TDZ001",  # Dealer Amount Difference Line
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 18,
                    "product_uom_qty": 1,
                    "dealer_price_unit": 20.0,  # API sent +, odoo change to -
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }


API2 (sale.order):

.. code-block:: python

    {
        "payload": {
            "status": "completed",  # Status: completed/return
            "partner_id": 1,  # Customer=Consignor (res.partner)
            "sunteen_dealer_id": 2,  # Dealer (res.partner)
            "workflow_process_id": "eCommerce Consignment Fix GP",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "sunteen_payment_method_id": 3,  # Payment Method (sunteen.payment.method)
            "sunteen_payment_provider_id": 34,  # Payment Provider (res.partner)
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                },
                {
                    "sunteen_parcel_number": "TDZ001",
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                },
                {
                    "sunteen_parcel_number": "TDZ001",  # Dealer Amount Difference Line
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 18,
                    "product_uom_qty": 1,
                    "dealer_price_unit": 20.0,  # API sent +, odoo change to -
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }


eCommerce - Consignment Var GP
##############################

API1 (sunteen.record.transportation.cost):

.. code-block:: python

    {
        "payload": {
            "status": "out of delivery",  # Status: out of delivery
            "partner_id": 1,  # Customer=Consignor (res.partner)
            "sunteen_dealer_id": 2,  # Dealer (res.partner)
            "workflow_process_id": "eCommerce Consignment Var GP",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "sunteen_payment_method_id": 3,  # Payment Method (sunteen.payment.method)
            "sunteen_payment_provider_id": 34,  # Payment Provider (res.partner)
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                },
                {
                    "sunteen_parcel_number": "TDZ001",
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                },
                {
                    "sunteen_parcel_number": "TDZ001",  # Dealer Amount Difference Line
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 18,
                    "product_uom_qty": 1,
                    "dealer_price_unit": 20.0,  # API sent +, odoo change to -
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }

API2 (sale.order):

.. code-block:: python

    {
        "payload": {
            "status": "completed",  # Status: completed/return
            "partner_id": 1,  # Customer=Consignor (res.partner)
            "sunteen_dealer_id": 2,  # Dealer (res.partner)
            "workflow_process_id": "eCommerce Consignment Var GP",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "sunteen_payment_method_id": 3,  # Payment Method (sunteen.payment.method)
            "sunteen_payment_provider_id": 34,  # Payment Provider (res.partner)
            "total": 500.0,  # Total

            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                },
                {
                    "sunteen_parcel_number": "TDZ001",
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                },
                {
                    "sunteen_parcel_number": "TDZ001",  # Dealer Amount Difference Line
                    "sunteen_partner_id": 14,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 18,
                    "product_uom_qty": 1,
                    "dealer_price_unit": 20.0,  # API sent +, odoo change to -
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }


Express
-------

* Express - Standard

Express - Standard
##################

API1 (sale.order):

.. code-block:: python

    {
        "payload": {
            "status": "out of delivery",  # Status: out of delivery
            "partner_id": 1,  # Customer (res.partner)
            "sunteen_merchant_id": 33,  # Merchant (res.partner)
            "sunteen_merchant_level": "ecom",  # Merchant Level: ecom/agent/franchise
            "sunteen_merchant_ownership": "my945",  # Merchant Ownership: my945/others
            "workflow_process_id": "Express Standard",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                    "type": "normal",  # Type: normal, cod
                    "cod_amount": 0,  # COD Amount

                },
                {
                    "sunteen_parcel_number": "TDZ002",
                    "sunteen_partner_id": 15,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                    "type": "cod",
                    "cod_amount": 20,
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }

API2 (sunteen.express.delivery.complete):

.. code-block:: python

    {
        "payload": {
            "status": "completed",  # Status : completed/return
            "partner_id": 1,  # Customer (res.partner)
            "sunteen_merchant_id": 33,  # Merchant (res.partner)
            "sunteen_merchant_level": "ecom",  # Merchant Level: ecom/agent/franchise
            "sunteen_merchant_ownership": "my945",  # Merchant Ownership: my945/others
            "workflow_process_id": "Express Standard",  # Automatic Workflow (sale.workflow.process)
            "date_order": "2020-01-31",  # Order Date
            "transaction_date": "",  # Transaction Date
            "total": 500.0,  # Total
            "order_line": [  # Sale Order Line
                {
                    "sunteen_parcel_number": "TDZ001",  # Parcel Number
                    "sunteen_partner_id": 14,  # Transporter (res.partner)
                    "sunteen_customer": "CUSTOMER1",  # Customer
                    "product_id": 16,  # Product (product.product)
                    "product_uom_qty": 1,  # Quantity
                    "dealer_price_unit": 100.0,  # Dealer Unit Price
                    "type": "normal",  # Type: normal, cod
                    "cod_amount": 0,  # COD Amount
                    "status": "completed",  # Status: completed/return

                },
                {
                    "sunteen_parcel_number": "TDZ002",
                    "sunteen_partner_id": 15,
                    "sunteen_customer": "CUSTOMER1",
                    "product_id": 17,
                    "product_uom_qty": 2,
                    "dealer_price_unit": 200.0,
                    "type": "cod",
                    "cod_amount": 20,
                    "status": "completed",
                },
            ]
        },
        "auto_create": {  # Optional for auto create master data
            "partner_id": [
                {"name": "Customer 1", "ref": "CUSTOMER1"}
            ],
            "product_id": [
                {"name": "Product 1", "default_code": "PRODUCT1"}
            ]
        }
    }

.. note::
    สำหรับ eCommerce การส่งข้อมูลจะเป็น 1 API ต่อ 1 Parcel Number ส่วน Express จะเป็น 1 API ต่อ หลาย Parcel Number


.. _job_queue:

> Job Queue
===========

Job Queue คืออะไร
------------------

Job Queue คือการหน่วงการทำงานของ process โดยแทนที่จะทำทันที ก็จะหน่วงและทำในภายหลัง

เช่น หากระบบต้นทางต้องการยิง API เข้ามา 1,000 รายการ ถ้าไม่มี Job Queue ระบบต้นทางจะต้องรอจนกว่ารายการก่อนหน้าจะทำงานเสร็จ
จึงจะยิงรายการถัดมาได้ ถ้า server ไม่สามารถทำงานได้เร็วพอก็จะเกิดปัญหาได้ ถ้าใช้ Job Queue ระบบต้นทางสามารถยิงข้อมูลเข้ามาได้ทั้งหมดอย่างรวดเร็วโดยไม่โหลด server
(เพราะยังไม่เกิดการทำงานจริง)

ข้อมูลการทำงานทั้งหมดจะเข้ามาพักที่ table :code:`queue.job` ของ Odoo ก่อน แล้วจึงทยอยทำงานพร้อมกันตามจำนวน **workers**

ข้อดีอีกประการหนึ่ง จากการที่ข้อมูลการทำงานได้เข้ามาพักไว้ก่อน ไม่ว่าการทำงานจะสำเร็จหรือไม่ ก็จะสามารถตรวจเช็คได้ในภายหลังที่ Odoo

ข้อดีของ Job Queue
------------------

1. เก็บรายการที่ยิงเข้ามาใน database table queue.job (ตั้งเวลาลบทิ้งได้หลังจากทำงานเสร็จและเวลาผ่านไปนาน)
2. Jobrunner: ทำงานกับ queue ที่เข้ามาได้อย่างทันที
3. Channels: แบ่งการทำงานของ workers ตาม channel, sub-channel ได้ เพื่อจำกัด worker บางตัวให้ทำงานบางอย่างไม่ปนกัน (ไม่แย่ง CPU)
4. Retries: มีการ retry งานที่ failed (บางครั้ง faile เพราะ update รายการเดียวกันพร้อมกัน ซึ่งทำอีกครั้งก็จะผ่านไปได้)
5. Retry Pattern: เช่น ให้ retry 3 ครั้งแรกห่างกัน 10 วินาที retry 5 ครั้งต่อไป ทุกๆ 1 นาที

.. note::
    สำหรับ 945 เราได้ตั้งค่าการทำงานแบบทั่วๆไป อนาคตสามารถเปลี่ยนแปลงได้ตามความเหมาะสม

Configurations
--------------

เพื่อให้ Job Queue ทำงานได้ ต้องมีการตั้งค่าที่ odoo config file.

.. code-block:: shell

    Using the Odoo configuration file:
    [options]
    (...)
    workers = 6
    server_wide_modules = web,queue_job

    (...)
    [queue_job]
    channels = root:2

.. note::
    ตัวอย่างนี้ให้ใช้ worker 2 ตัวจากทั้งหมด 6 ตัว ทำงานกับ channel = root

Usage
-----

.. image:: images/job_queue1.png
    :align: center

.. nextslide::

1. Job Menu: แต่ละ API ที่ถูกยิงเข้ามาจะเข้ามาสร้าง Job โดยจะเริ่มต้นที่ status = Pending
2. Job Status: Pending = รอ, Enqueue = เข้าคิว, Started = เริ่มทำงาน, Done = เสร็จ, Failed = ไม่สำเร็จ
3. Task: รายละเอียดของ function call ที่จะทำ โดยมี input data ทั้งหมดที่ต้องใช้
4. Function / Channel: Function นี้ทำงานผ่าน Channel ใหน เช่น root.sunteen_api ซึ่งเป็น sub-channel ของ root (จะใช้ร worker ่วมกับ root หากไม่ประกาศแยกไว้)
5. Retry: จำนวนครั้งที่ retry
6. Result: ผลลัพธ์ที่ได้ หากเกิด Error จะแสดงเป็นข้อความ exception และเปลี่ยนสถานะเป็น status = Failed (และ retry)

.. _sale_automatic_workflow:

> Sale Automatic Workflow
=========================

Sale Automatic Workflow คืออะไร
---------------------------------

Sale Automatic Workflow คือ กระบวนการการดำเนินงานโดยอัตโนมัติที่เกี่ยวข้องกับการขาย ตัวอย่างกระบวนการการดำเนินงานที่เกี่ยวข้องกับการขาย เช่น
การ Confirm Sale Order, การสร้าง Invoice จาก Sale Order และการ Validate Invoice ของ Sale Order เป็นต้น
โดยกระบวนการการดำเนินงานโดยอัตโนมัติที่เกี่ยวข้องกับการขาย สามารถทำงานได้ใช้โมดูล
sale_automatic_workflow และ sale_automatic_workflow_job ของ OCA

โมดูล sale_automatic_workflow เป็นโมดูลที่ใช้ในการสร้างและตั้งค่าการทำงานของ Automatic Workflow
ที่จะนำมาใช้ในกระบวนการขาย ซึ่งเหมาะสำหรับระบบที่มีการทำงานโดยการ Interface ผ่าน API เข้ามาที่ Sale Order ใน Odoo

โมดูล sale_automatic_workflow_job เป็นโมดูลที่รวมการทำงานของ sale_automatic_workflow และ job_queue เข้าด้วยกัน
ทำให้ระบบสามารถจัดลำดับการทำงานของ Automatic Workflow ได้

Configurations
----------------

การตั้งค่า Sale Automatic Workflow แบ่งออกเป็น 3 ส่วน ดังนี้

#. Order Configuration
#. Workflow Options
#. 945 Workflow Options

Order Configuration
#####################

#. **Shipping Policy** คือ วิธีการจัดส่งสินค้า ซึ่งแบ่งได้ดังนี้

    * Deliver all products at once คือ ส่งสินค้าทั้งหมดที่ลูกค้าต้องการในครั้งเดียว
    * Deliver each product when available คือ ส่งสินค้าเท่าที่มีในคลัง(ตามที่ระบุในระบบ)

#. **Sales Team** คือ ทีมที่ขายสินค้าได้

Workflow Options
#################

#. **Validate Order** คือ ต้องการให้ Confirm Sale Oder หรือไม่ โดยสามารถจำกัด Domain ของ Sale Order ที่ต้องการ Confirm ได้ที่ **Order Filter**
#. **Confirm and Transfer Picking** คือ ต้องการให้ส่งของและ Validate Delivery Order หรือไม่
#. **Create Invoice** คือ ต้องการให้สร้าง Invoice หรือไม่ โดยสามารถจำกัด Domain ของ Sale Order ที่ต้องการสร้าง Invoice ได้ที่ **Create Invoice Filter**
#. **Validate Invoice** คือ ต้องการให้ Validate Invoice หรือไม่ โดยสามารถจำกัด Domain ของ Sale Order ที่ต้องการ Validate ได้ที่ **Validate Invoice Filter**
#. **Sale Done** คือ ต้องการ Locked Sale Order หลังจาก Confirm Sale Oder หรือไม่
#. **Force Invoice Date** คือ ต้องการให้ Invoice Date ใน Invoice เป็นวันเดียวกับ Order Date ใน Sale Order หรือไม่
#. **Invoice Service on delivery** คือ ต้องการให้บันทึกจำนวน Deliveried ของสินค้าประเภทบริการใน Sale Order หลังจากสร้าง Invoice หรือไม่
#. **Sales Journal** คือ Journal ที่จะใช้ใน Invoice
#. **Warning Message** คือ ข้อความที่จะขึ้นแจ้งเตือน เมื่อเลือก Automatic Workflow นี้ ใน Sale Order

945 Workflow Options
#####################

#. **Allow Compute Sale Order Line** คือ ต้องการให้คำนวณรายได้อื่นๆ เช่น รายได้ค่าขนส่ง ใน Sale Order หรือไม่
#. **Record Transportation Cost** คือ ต้องการสร้าง Transportation Cost (TC) หรือไม่
#. **Record Delivery Complete** คือ ต้องการสร้าง Delivery Complete (DC) หรือไม่
#. **Consignment Payable** คือ วิธีการคำนวณเจ้าหนี้การค้าฝากขาย ซึ่งจะเท่ากับ Account Payable Transfer (APT) โดยสามารถเลือกได้ ดังนี้

    * None คือ ไม่คำนวณ Consignment Payable
    * Fixed Cost คือ คำนวณโดยที่ทราบต้นทุนที่แน่นอน เช่น เมื่อขายสินค้า A ได้ จะต้องจ่ายเงินให้ผู้ฝากขาย 10 บาท/หน่วย
    * Variance Cost คือ คำนวณโดยที่ไม่ทราบต้นทุนที่แน่นอน เช่น เมื่อขายสินค้า B ได้ ผู้ฝากขายจะได้รับเงินตามรายได้หลังหักค่าใช้จ่ายต่างๆแล้ว

945's Configs
--------------

ปัจจุบัน 945 มีการใช้ Sale Automatic Workflow ทั้งหมด 4 แบบ ดังนี้

#. eCommerce Standard
#. eCommerce Consignment Fix GP
#. eCommerce Consignment Var GP
#. Express Standard

eCommerce Standard
###################

.. image:: images/sale_automatic_workflow/automatic_workflow_ecom_std.png
    :align: center

.. nextslide::

eCommerce Consignment Fix GP
#############################

.. image:: images/sale_automatic_workflow/automatic_workflow_ecom_consign_fix.png
    :align: center

.. nextslide::

eCommerce Consignment Var GP
#############################

.. image:: images/sale_automatic_workflow/automatic_workflow_ecom_consign_var.png
    :align: center

.. nextslide::

Express Standard
#################

.. image:: images/sale_automatic_workflow/automatic_workflow_express_std.png
    :align: center

.. _account_move_template:

> Account Move Template
=======================

Account Move Template คืออะไร
---------------------------------

Configurations
--------------

945's Templates
---------------

* Standard
* ???

.. _compute_helper:

> Compute helper
================

.. _unit_test:

> Unit Test
===========

What is unit test?
------------------
