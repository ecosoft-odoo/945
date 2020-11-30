> Deferred Revenue
====================

#. สร้าง Business Unit ใน Odoo
#. Deferred Revenue
#. Payment Gateway Settlement
#. Receive Input VAT

สร้าง Business Unit ใน Odoo
------------------------------

.. image:: images/deferred_revenue/business_unit.png
    :align: center

.. nextslide::

#. เข้าไปที่ 945 > Business Unit
#. กด Create แล้วใส่ข้อมูลของ Business Unit เพื่อสร้าง Business Unit

Deferred Revenue
------------------

.. image:: images/deferred_revenue/deferred_revenue.gif
    :align: center

.. nextslide::

#. เข้าไปที่ 945 > Operations > Deferred Revenue
#. กด Create แล้วเลือก Business Unit และใส่จำนวนเงิน
#. กด Create Move แล้วตรวจสอบข้อมูลในการบันทึกบัญชีให้ถูกต้อง หลังจากนั้นกด Create Journal Entry เพื่อสร้าง Journal Entry

.. nextslide::

**ตัวอย่างการบันทึกบัญชี**

.. image:: images/deferred_revenue/je_deferred_revenue.png
    :align: center

Payment Gateway Settlement
---------------------------

.. image:: images/deferred_revenue/payment_gateway_settlement.gif
    :align: center

.. nextslide::

#. เข้าไปที่ 945 > Operations > Payment Gateway Settlement
#. กด Create แล้วเลือก Journal Entry และใส่จำนวนเงินใน Total Amount, MDR, VAT
#. กด Create Move แล้วตรวจสอบข้อมูลในการบันทึกบัญชีให้ถูกต้อง หลังจากนั้นกด Create Journal Entry เพื่อสร้าง Journal Entry

.. nextslide::

**ตัวอย่างการบันทึกบัญชี**

.. image:: images/deferred_revenue/je_payment_gateway_settlement.png
    :align: center

Receive Input VAT
-------------------

.. image:: images/deferred_revenue/receive_input_vat.gif
    :align: center

.. nextslide::

#. เข้าไปที่ 945 > Operations > Payment Gateway Settlement
#. เลือกรายการที่ต้องการบันทึก Input VAT
#. ไปที่ Action > Receive Input VAT ระบบจะรวบรายการต่างๆไปไว้ที่ 945 > Operations > Receive Input VAT
#. เข้าไปที่ 945 > Operations > Receive Input VAT
#. กด Create Move ที่รายการที่ต้องการ แล้วตรวจสอบข้อมูลในการบันทึกบัญชีให้ถูกต้อง หลังจากนั้นกด Create Journal Entry เพื่อสร้าง Journal Entry

.. nextslide::

**ตัวอย่างการบันทึกบัญชี**

.. image:: images/deferred_revenue/je_receive_input_vat.png
    :align: center
