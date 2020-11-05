945 Functional Manual
=====================

Overview
--------

ส่วนต่างๆ 3 ส่วนที่ประกอบกันขึ้นมาเป็นระบบ 945

1. ส่วน Automatic Workflow :: ส่วนนี้เป็นการบันทึก Operations ต่างๆที่เกิดขึ้นจากระบบหน้าบ้านทั้งหมด เช่นการขาย ค่าขนส่ง การส่งของ โดยทั้งหมดจะยิงผ่าน API
   เข้ามาทางหน้าต่างของ Odoo/945 ที่เกี่ยวข้อง ทำให้เกิดการบันทึกบัญชีตามกระบวนการ และไปจบที่การตั้งหนี้เพื่อรอการ Manual เคลียร์โดยนักบัญชี
2. ส่วน Manual Operations :: เป็นการทำงานโดยนักบัญชีผ่านหน้าจอเพื่อทำงานต่างๆของ Odoo เช่น การจ่ายหนี้ การจ่าย Commission การรับเงินเข้าธนาคาร
3. ส่วน Reporting :: ข้อมูลด้านบัญชีทั้งหมดที่เกิดขึ้นในข้อ 1 และ 2 จะถูกนำมาวิเคราะห์ผ่านระบบรายงานของ Odoo หรือผ่านระบบ BI หลังบ้านอื่นๆ

.. note::
    ส่วนนี้ทำงานอัตโนมัติทั้งหมดโดยผู้ใช้แทบไม่ต้องทำอะไร เอกสารชุดนี้จะแนะนำส่วนนี้พอสังเขปเพื่อให้เห็นการทำงานในระบบ

Automatic Workflow
==================


Manual Operations
=================

Clear Account Receivable
------------------------

เมื่อได้รับ Statement จากธนาคารว่าได้รับเงินเข้ามาจากลูกหนี้ของทาง 945 และต้องการเคลียร์ลูกหนี้ที่ค้าง

1. ตั้งค่า Mass Automatic Reconcile สำหรับการเคลียร์ลูกหนี้
2. นำเข้า Statement ตามที่ได้รับแจ้งจากธนาคาร
3. ทำการ Reconcile และตรวจสอบผลลัพธ์

1. การตั้งค่า Mass Automatic Reconcile
#############################################

Menu: Accounting > Accounting > Actions > Mass Automatic Reconcile

.. image:: images/reconcile_ar/mass_reconcile_ar.png
    :align: center


2. นำเข้า Statement ตามที่ได้รับแจ้งจากธนาคาร
############################################

1. เตรียม Excel โดยใช้ข้อมูลจาก statement ที่ได้รับมา
2. ที่เมนู Journal Entries สร้างรายการใหม่ ซึ่งจะทำหน้าที่เป็น Payment Entry
3. คลิกเมนู Action / Import Excel
4. ตรวจทานให้เรียบร้อยจึงก่อน Post

.. nextslide::

ค้นหา journal items ที่สนใจเพื่อ export มาเป็นค่าเริ่มต้น

.. note::
    ขั้นตอนนี้เป็นขั้นตอนพิเศษเพื่อได้ข้อมูลทดสอบ การทำงานจริงข้อมูลจะมาจาก statement ตรงๆ

.. image:: images/reconcile_ar/1_goto_journal_items.png
    :align: center

.. nextslide::

โดยสามารถค้นหารายการที่ยังไม่ได้รับการเคลียร์ ด้วยด้วย Filter ดังต่อไปนี้

1. Filtered / Group By
    * Posted
    * Unreconciled
    * Account = ลูกหนี้การค้า
    * Partner = Transporter (เช่น DHL, SCB, etc.)
2. ถ้าต้องการเราสามารถสร้าง Save Filter เอาไว้ใช้งานได้

.. image:: images/reconcile_ar/2_search_unreconciled_ar.png
    :align: center

.. nextslide::

เลือกรายการที่สนใจ เพื่อนำข้อมูลออกมาที่ Excel

.. image:: images/reconcile_ar/3_export_excel.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ar/4_export_excel_switch_drcr.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ar/5_export_excel.png
    :align: center

.. nextslide::

เพิ่มรายการในขา Bank เพื่อให้ Journal Entry นี้ดุล

.. image:: images/reconcile_ar/6_excel_add_bank_amount.png
    :align: center

.. note::
    ระบบจะใช้ sheet = Journal Items ในการอัพเดทข้อมูล

.. nextslide::

สร้าง Journal Entry ใหม่ ทำหน้าที่เป็นเสมือนกับ Payment Entry

.. image:: images/reconcile_ar/7_create_payment_entry.png
    :align: center

.. nextslide::

เพิ่มรายการด้วยการ Import Excel ตามที่ได้เตรียมไว้

.. image:: images/reconcile_ar/8_import_excel.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ar/9_post_payment_entry.png
    :align: center

.. note::
    ต้องตรวจสอบให้แน่ใจ แล้วจึงค่อย Post


3. ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile เลือก Profile = Customer Payment
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปให้ทำการ Reverse Entry

.. nextslide::

ไปที่เมนู Mass Automatic Reconcile แล้วเลือก/สร้าง Profile = Customer Payment
แล้วจีง Start Reconcile

.. image:: images/reconcile_ar/10_click_cust_payment_mass_reconcile.png
    :align: center

.. nextslide::

ระบบจะมีการเก็บประวัติของการ Reconcile เอาไว้ สามารถคลิกเพื่อตรวจสอบได้

.. image:: images/reconcile_ar/11_review_cust_payment_reconcile_history.png
    :align: center

.. note::
    เราสามารถตั้ง Schedule Job ให้ Start Auto Reconciliation ได้อย่างอัตโนมัติหากต้องการ

.. nextslide::

รีวิวรายการที่เกิดขึ้น ให้สังเกตุที่ Reconcile ID ที่ระบบได้สร้างขึ้นเพื่อ Match Dr/Cr ล้างกัน

.. image:: images/reconcile_ar/12_review_cust_payment_reconciled_items.png
    :align: center

.. nextslide::

หากต้องการยกเลิกสิ่งที่ได้ทำไป ให้ทำการ Reverse Entry ระบบจะสร้างอีก Journal Entry เพื่อล้างตัวเอง

.. image:: images/reconcile_ar/13_reverse_entry.png
    :align: center

End.

Clear AP Commission
-------------------

การบันทึกคู่บญชีอัตโนมัติของ Delivery Complete ได้ทำให้เกิดค่าคอมมิชชั่น ซึ่งทาง 945 ต้องทำจ่ายให้กันผู้ได้รับส่วนแบ่ง

1. ตั้งค่า Mass Automatic Reconcile สำหรับการเคลียร์เจ้าหนี้
2. เลือกรายการที่ต้องทำจ่าย โดยดูตามวันที่ (ศุกร์ถัดไป) และนำไปสร้าง Journal Entry สำหรับการจ่ายเงิน
3. ออก Withholding Tax Cert ให้กับผู้รับเงิน
4. ทำการ Reconcile และตรวจสอบผลลัพธ์


1. การตั้งค่า Mass Automatic Reconcile
#############################################

Menu: Accounting > Accounting > Actions > Mass Automatic Reconcile

สร้าง Profile สำหรับ Supplier Payment

.. image:: images/reconcile_ap_commission/mass_reconcile_ap.png
    :align: center

.. note::
    เนื่องจากเราไม่ได้แบ่ง Account Code เป็็นเรื่องย่อยๆ
    Mass Reconcile นี้อาจถูกใช้่ร่วมกับการจ่ายเงินด้านอื่นๆที่ใช้ AP Account เดียวกันด้วย

2. เลือกรายการที่ต้องทำจ่าย
############################################

สำหรับ Commission จะดูตามวันที่ (เช่น ศุกร์ถัดไป) โดยสามารถค้นหาที่เมนู Journal Items ด้วย Filter ดังต่อไปนี้

1. Filtered / Group By
    * Posted
    * Unreconciled
    * Account = เจ้าหนี้การค้า
    * Journal Item's Label = Account Payable (Commission)
    * Group by
        * Due Date
        * Partner
2. เลือกรายการที่ต้องการจ่ายค่า Commission ให้และทำการ Export Excel (ระบบจะสลับ Dr/Cr ตั้งให้)

.. nextslide::

จากรายการที่เลือก ให้เลือก Action > Export Excel

.. image:: images/reconcile_ap_commission/1_find_commission_items.png
    :align: center

.. nextslide::

จากค่าเริ่มต้นที่ได้ ให้เพิ่มบรรทัด Bank และ WHT (คำนวนเอง) ให้ดุลกัน

.. image:: images/reconcile_ap_commission/2_prepare_excel.png
    :align: center

.. nextslide::

สร้าง Journal Entry ใหม่ ทำหน้าที่เป็นเสมือนกับ Payment Entry แล้วจึงสร้างรายการด้วยการ Import Excel

.. image:: images/reconcile_ap_commission/3_review_and_post.png
    :align: center

.. note::
    ต้องตรวจสอบให้แน่ใจ แล้วจึงค่อย Post

.. nextslide::

3. ออก Withholding Tax Cert ให้กับผู้รับเงิน
################################################

จาก Journal Entry ในขั้นตอนก่อน เลือก Action > Create Withholding Cert

.. image:: images/reconcile_ap_commission/4_create_wht_cert.png
    :align: center

.. nextslide::

ระบบจะช่วยสร้าง Cert จากรายการที่บันทึก Account Code - WHT

.. image:: images/reconcile_ap_commission/5_create_wht_cert.png
    :align: center

.. nextslide::

ให้ผู้ใช้งานกรอกข้อมูลให้ครบแล้วกด Save ตรวจสอบความถูกต้องแล้วกดปุ่ม Done

.. image:: images/reconcile_ap_commission/6_create_wht_cert.png
    :align: center

.. nextslide::

เลือก Print > Withholding Tax Cert เป็น PDF

.. image:: images/reconcile_ap_commission/7_print_wht_cert.png
    :align: center

.. note::
    ผู้ใช้งานสามารถดู Certificate. ทั้งหมดในภายหลังได้ที่เมนู Accounting > Vendors > WT Certificates

4. ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile เลือก Profile = Supplier Payment
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปที่ Journal Entry ให้ทำการ Reverse Entry

.. nextslide::

.. image:: images/reconcile_ap_commission/8_reconcile_ap_commission.png
    :align: center

.. note::
    เราสามารถตั้ง Schedule Job ให้ Start Auto Reconciliation ได้อย่างอัตโนมัติหากต้องการ

End.

Clear AP Transporter
--------------------

#. ย้ายเจ้าหนี้ประมาณการค่าขนส่งเป็นเจ้าหนี้ ด้วย Excel
    #. ตั้งค่า Mass Automatic Reconcile สำหรับการเคลียร์เจ้าหนี้ (ถ้ายังไม่มี)
    #. เลือกรายการที่ต้องทำจ่าย (ครบกำหนดวันที่ 15 ของ 2 เดือนหลัง) และ Export Excel ตั้งต้น
    #. เทียบกับค่าขนส่งจริงเพื่อย้ายค่าประมาณการเข้าเจ้าหนี้ โดยมีส่วนต่างค่าขนส่ง และ Import Excel เพื่อสร้าง JE
    #. ทำการ Reconcile และตรวจสอบผลลัพธ์
#. การเคลียร์เจ้าหนี้ โดยจะมีการหักภาษี ณ ที่จ่าย ภงด 53 ไว้ 1% ด้วย Journal Entry
    #. สร้าง Journal Entry ด้วย Template ที่เตรียมไว้ล่วงหน้า
    #. Manual Reconcile เพื่อเคลียร์เจ้าหนี้ขนส่ง
    #. ออกเอกสาร Withholding Tax Cert.

1.1 การตั้งค่า Mass Automatic Reconcile
#############################################

Menu: Accounting > Accounting > Actions > Mass Automatic Reconcile

สร้าง Profile สำหรับ Supplier Payment (ประมาณการค่าขนส่ง)

.. image:: images/reconcile_ap_transport/mass_reconcile_ap.png
    :align: center

.. note::
    เนื่องจากเราไม่ได้แบ่ง Account Code เป็็นเรื่องย่อยๆ
    Mass Reconcile นี้อาจถูกใช้่ร่วมกับการจ่ายเงินด้านอื่นๆที่ใช้ AP Account เดียวกันด้วย

1.2 เลือกรายการที่ต้องทำจ่าย
############################################

รายการประมาณการค่าขนส่ง จะดูตามวันที่ 15 โดยสามารถค้นหาที่เมนู Journal Items

1. Filtered / Group By (หรือด้วย Favorite Filter = ชำระค่าขนส่ง)
    * Filter: Posted, Unreconciled, Account = ประมาณการ - เจ้าหนี้การค้า
    * Group by: Due Date, Partner

.. image:: images/reconcile_ap_transport/1_find_transport_items.png
    :align: center

2. เลือก Journal Items ทั้งหมดของ Transporter ที่ต้องการจ่ายทำรายการ
3. Actions > Export Excel (เลือก Template = JE for Clear Transport)

1.3 เทียบกับค่าขนส่งจริงเพื่อย้ายค่าประมาณการเข้าเจ้าหนี้
############################################################

ใส่ข้อมูลจริงที่ Excel Sheet = External จะได้ข้อมูลเบื้องต้น ทำการปรับให้พร้อมสำหรับ Import ตามข้อ 1-3

.. image:: images/reconcile_ap_transport/2_prepare_excel.png
    :align: center

.. nextslide::

สร้าง Journal Entry ใหม่ ทำหน้าที่ตั้งเจ้าหนี้ Vendor Bill แล้วจึงสร้างรายการด้วยการ Import Excel

.. image:: images/reconcile_ap_transport/3_review_and_post.png
    :align: center

.. note::
    ต้องตรวจสอบให้แน่ใจ แล้วจึงค่อย Post

1.4 ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile
    เลือก Profile = 212107 ประมาณการ - เจ้าหนี้การค้า
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปที่ Journal Entry ให้ทำการ Reverse Entry

.. note::
    เราสามารถตั้ง Schedule Job ให้ Start Auto Reconciliation ได้อย่างอัตโนมัติหากต้องการ

2.1 สร้าง Journal Entry ด้วย Template - Pay for AP Transporter
##################################################################

Menu: Accounting > Accounting > Create Entry from Template

ให้เลือก Template ให้ถูกต้องและกดปุ่ม Next

.. image:: images/reconcile_ap_transport/4_create_je_ap_transporter.png
    :align: center

.. nextslide::

ใส่ข้อมูลตามรูป แล้วกดปุ่ม Create Journal Entry

.. image:: images/reconcile_ap_transport/5_create_je_ap_transporter.png
    :align: center

.. nextslide::

ระบบจะช่วยสร้าง Journal Entry ให้ พร้อมๆกับช่วยคำนวน WHT 1% ด้วย

.. image:: images/reconcile_ap_transport/6_je_ap_transporter.png
    :align: center

2.2 Manual Reconcile ลูกหนี้ค่าขนส่ง
#########################################

Menu: Accounting > Accounting > Actions > Reconciliation

เราสามารถใช้หน้าต่างนี้ทำการ Reconcile รายการทั่วๆไป ในกรณีนี้เราต้องการเคลียร์เจ้าหนี้ของ Transporter

.. image:: images/reconcile_ap_transport/7_manual_reconcile.png
    :align: center

2.3 ออก Withholding Tax Cert ให้กับผู้รับเงิน
################################################

1. จาก Journal Entry ในขั้นตอนก่อน เลือก Action > Create Withholding Cert
2. ระบบจะช่วยสร้าง Cert จากรายการที่บันทึก Account Code - WHT
3. ให้ผู้ใช้งานกรอกข้อมูลให้ครบแล้วกด Save ตรวจสอบความถูกต้องแล้วกดปุ่ม Done
4. เลือก Print > Withholding Tax Cert เป็น PDF
