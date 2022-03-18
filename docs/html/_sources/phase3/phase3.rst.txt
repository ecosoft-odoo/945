945 Phase 3 (CRs)
====================

945 Phase 3 Change Request Scope
---------------------------------------

ในเฟสนี้จะเป็นการแก้ไขระบบเพื่อตอบความต้องการเพิ่มเติมเป็นเรื่องๆ แต่ละข้อจะเป็นเอกเทศจากข้ออื่นๆ

1. ส่งของไปผิดต้องการเลือกของให้ถูกต้องเพื่อให้สต๊อกถูกต้อง
2. Customer Refund เฉพาะบางบรรทัด
3. Multi Invoice's Send & Print สามารถเลือก Printed Form ได้
4. Work Acceptance & Partner Evaluation
5. ที่หน้าต่าง Journal Entry อยากให้ระบบช่วย Create Withholding Tax Cert ให้อัตโนมัติ โดยไม่ต้องกดเอง
6. Import Bank Statement แล้ว ต้องการให้ระบบ Matching ให้ด้วย Date + Amount
7. Asset Reports ให้มี Filter By: Sell, Disposed
8. รายงาน Aging AR/AP แบ่ง period ละ 90 ไปจนถึง 360 วัน
9. Withholding Tax Cert, ต้องการเลือก multi WT Cert แล้วพิมพ์เป้น 1 PDF
10. Unbuild Manufacturing Order
11. New "Invoice Readonly" group for customer service users
12. Sales Order จาก Scrap Location


1. ส่งของไปผิดต้องการเลือกของให้ถูกต้องใน
---------------------------------------

ในหัวข้อนี้ ไม่ได้มีการปรับแต่งระบบ แต่เป็นการแนะนำการทำงานได้สองวิธี

1. Return DO เฉพาะบรรทัดที่ผิดพลาด แล้วจึงสร้าง DO ใหม่แบบ manual และเขียน Note อ้างอิงถึงเอกสารที่ผิดพลาด
2. ใช้วิธีการปรับสต๊อก (Inventory Adjustment)

Note: ทีมสรุปใช้ข้อ (1)

2. Customer Refund เฉพาะบางบรรทัด
---------------------------------------

Extra addons: account_invoice_refund_line_selection

1. ที่ Invoice (post, paid)
2. กดปุ่ม Create Credit Note
3. เลือกบรรทัดที่ต้องการ Refund

.. image:: images/cr_2.png
    :align: center


3. Multi Invoice's Send & Print สามารถเลือก Printed Form ได้
--------------------------------------------------------------------

ข้อนี้เป็นการตั้งค่า email template โดยให้เลือก Prined Form ที่ต้องการ

1. เลือกหลาย Inovice และกดปุ่ม Send & Print
2. สามารถเลือก Print และ/หรือ Email
3. เลือก Email Template ที่ต้องการ
4. ใน Template สามารถตั้งค่าให้ใช้ Printed Form ที่ต้องการได้

.. image:: images/cr_3.png
    :align: center


4. Work Acceptance & Partner Evaluation
----------------------------------------------------------

Extra Addons: purchase_work_acceptance, purchase_work_acceptance_evaluation

1. หลังจากติดตั้ง addons แล้วให้ตั้งค่าเพื่อเปิดใช้งานโมดูล Work Acceptance

.. image:: images/cr_4_1.png
    :align: center

2. เพื่อให้ใช้งานกับกรณี Lot ได้ถูกต้อง ที่ Operation Type คลัง 945: Receipts ให้เซต Pre-filled Detailed Operations

.. image:: images/cr_4_2.png
    :align: center

3. ตั้งค่า Evaluation Criterias

.. image:: images/cr_4_3.png
    :align: center

4. ที่ Purchase Order กดปุ่ม Create WA เพื่อสร้างเอกสารตรวจรับ (ก่อนส่งของหรือวางบิล)

.. image:: images/cr_4_4.png
    :align: center


5. ที่หน้าต่าง WA ให้กรอกจำนวนสินค้าที่ตรวจรับ และกรอกการประเมินตาม Evaluation Criteria แล้วกด Accept

.. image:: images/cr_4_5.png
    :align: center

.. image:: images/cr_4_6.png
    :align: center

6. เลือก WA ที่ตรวตรับไปแล้วการการรับของและวางบิล ระบบจะใช้ Quantity ตามที่ตรวจรับ

.. image:: images/cr_4_7.png
    :align: center


5. ที่หน้าต่าง Journal Entry อยากให้ระบบช่วย Create Withholding Tax Cert ให้อัตโนมัติ
---------------------------------------------------------------------------------------

Extra Addons: sunteen_auto_create_wht_cert

เราสามารถตั้งค่าให้เฉพาะ Posted JE ของบาง Journal เท่านั้นที่มีการสร้าง Auto Create WHT Cert 

1. ตั้งค่า Auto Create WHT Cert ที่ Journal เช่น Wallet Top Upgrade

.. image:: images/cr_5_1.png
    :align: center

2. เมื่อ Post JE ของ Journal ดังกล่าว ระบบจะไปดูที่ Partner’s Withholding Tax เพื่อใช้เป็นค่าในการสร้าง Cert

Note: ถ้าไม่มีการตั้งค่าเริ่มต้นที่ถูกต้องระบบจะไม่มีการสร้าง Cert

.. image:: images/cr_5_2.png
    :align: center


6. Import Bank Statement แล้ว ต้องการให้ระบบ Matching ให้ด้วย Date + Amount
----------------------------------------------------------------------------

Extra Addons: sunteen_payment_move_line_stamp_date

1. ตั้งค่า Reconcile Models ให้ Partner Is Set & Matches = False (ไม่ต้องการใช้ Partner เพื่อ matching)

.. image:: images/cr_6_1.png
    :align: center

2. ในทุก Payment’s JE, ระบบจะstamp payment date = @dd/mm/yy เพิ่มไว้ที่ Journal Item เพื่อใช้สำหรับการ match Date

.. image:: images/cr_6_2.png
    :align: center

3. เมื่อมีการสร้าง Bank Statement Import, ให้ใส่ Date mm/dd/yyyy ไว้ที่ Label ด้วย

.. image:: images/cr_6_3.png
    :align: center

4. เมื่อมีการ Reconcile ระบบจะสามารถ auto match ด้วย Date + Amount

.. image:: images/cr_6_4.png
    :align: center


7. Asset Reports ให้มี Filter By: Sell, Disposed
----------------------------------------------------------------------------

Extra Addons: asset_report_filter

เมนู Accounting > Reporting > Management > Asset Reports, เพิ่ม Filter ในรายงาน

.. image:: images/cr_7.png
    :align: center


8. รายงาน Aging AR/AP แบ่ง period ละ 90 ไปจนถึง 360 วัน
----------------------------------------------------------------------------

Extra Addons: account_aged_balance_90

เมนู Accounting > Reporting > Partner Reports > Aged Receivables / Payable

.. image:: images/cr_8.png
    :align: center


9. Withholding Tax Cert, เลือก multi WT Cert แล้วพิมพ์เป้น 1 PDF
----------------------------------------------------------------------------

Extra Addons: l10n_th_withholding_tax_cert_form_sum

เลือก multi WHT Cert แล้วพิมพ์ ระบบจะรวมเงินให้
แต่ WHT Cert จะต้องไม่ปนกัน ถ้าปนกันระบบจะแสดงคำเตือน

.. image:: images/cr_9.png
    :align: center


10. Unbuild Manufacturing Order
----------------------------------------------------------------------------

เลือก MO และ Quantity ที่ต้องการ Unbuild เพื่อ Unpack

.. image:: images/cr_10.png
    :align: center

11. New "Invoice Readonly" group for customer service users
----------------------------------------------------------------------------

หัวข้อนี้จะเป็นการเพิ่มกลุ่มใหม่ที่ Read Invoice ได้อย่างเดียว

1. สร้างกลุ่มใหม่ “Invoice Readonly, และเพิ่มสิทธิ์ Objects ตามรูป

.. image:: images/cr_11_1.png
    :align: center

2. เพิ่มเมนู Accounting

.. image:: images/cr_11_2.png
    :align: center

3. เพิ่ม User ที่ต้องการสิทธิ์นี้ (หรือที่หน้าต่าง users)

.. image:: images/cr_11_3.png
    :align: center

4. ผู้ใช้งานจะมีสิทธิ์ View Invoice และ Print Forms เท่านั้น

.. image:: images/cr_11_4.png
    :align: center



12. Sales Order จาก Scrap Location
----------------------------------------------------------------------------

เพื่อให้ Sales Order สามารถเลือกได้ว่าจะส่งของออกจาก Scrap Location (new route) เราจะต้องตั้งค่า Route ใหม่ให้ระบบ

1. เปิดการใช้งาน Inventory Routing

.. image:: images/cr_12_1.png
    :align: center

2. เมนู Inventory > Configurations > WH Management > Routes, สร้าง Route ใหม่โดยการ Copy จาก Route เดิม

.. image:: images/cr_12_2.png
    :align: center

3. ตั้งค่า route ใหม่โดยใช้ Source Location = Scrap

(1)  เลือก Sales Order Lines = True เพื่อเปิด Option นี้สำหรับ Sales Order
(2)  เลือก Source Location = Scrap Location เพื่อให้ส่งของออก

.. image:: images/cr_12_3.png
    :align: center

4. เมื่อใช้งาน Sales Order ระบบจะมีให้เลือก Route และเมื่อ Delivery Order สร้าง จะส่งของจาก Scrap Location

.. image:: images/cr_12_4.png
    :align: center
