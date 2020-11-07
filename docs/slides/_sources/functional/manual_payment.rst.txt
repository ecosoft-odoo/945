> Manual Payment
=================
ระบบการเคลียร์หนี้ AR/AP

Mass Reconciliation
--------------------

ในระบบของ 945 เพื่อให้การเคลียร์หนี้ (reconcile AR/AP) ทำได้อย่างรวดเร็ว เราจะใช้ฟังก์ชั่น **Mass Automatic Reconcile**

**Menu:** Accounting > Accounting > Actions > Mass Automatic Reconcile

.. image:: images/all_mass_reconcile.png
    :align: center

.. nextslide::

ในทุกๆ Profile เราจะมีการเซตค่าตามรูป (ต่าง Account Code)

.. image:: images/sample_mass_reconcile.png
    :align: center

การทำงานของหน้าต่าง Mass Automatic Reconcile
#################################################

1. ตั้งชื่อ Profile และเลือก Account Code
2. ตั้งกฏการ Reconcile ด้วย Partner และ Parcel ID
3. แสดงรายการ journal items ที่ใช้ Account เดียวกันจะอาจถูก reconcile หากจับคู่ได้ตรงตามกฏ
4. ทำการ Reconcile
5. ดูรายการ Reconcile ล่าสุดที่เกิดขึ้น (ดูย้อนหลังได้ที่ tab History)

.. note::
    เราสามารถตั้งค่า Scheduled Job ให้ทำงานเป็นช่วงๆแบบอัตโนมัติได้ด้วย

Clear AR Provider
-----------------

เมื่อได้รับ Statement จาก Transporter ว่าได้รับเงินเข้ามาจากลูกหนี้ของทาง 945 และต้องการเคลียร์ลูกหนี้ที่ค้างจ่าย

1. เตรียมไฟล์ Excel จากระบบเพื่อการตรวจสอบ (optional)
2. นำเข้า Statement ตามที่ได้รับแจ้งจาก Transporter
3. ทำการ Reconcile และตรวจสอบผลลัพธ์

1. เตรียมไฟล์ Excel จากระบบเพื่อการตรวจสอบ
###########################################

Export journal items ที่สนใจมาเป็นค่าตั้งต้นหากต้องการเทียบกับ statement ที่ได้รับมา

1. กรองรายการด้วย Favorite = **AR Provider**
2. เลือกรายการที่ต้องการ Export
3. คลิกเมนู Action > Export Excel โดยเลือก Template = **AR Provider**

.. image:: images/reconcile_ar_provider/1_search_journal_items.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ar_provider/2_export_excel_wizard.png
    :align: center

.. note::
    ขั้นตอนนี้เป็นขั้นตอนพิเศษเพื่อได้ข้อมูลทดสอบ การทำงานจริงข้อมูลจะมาจาก statement ตรงๆ

.. nextslide::

ตัวอย่าง Excel ของการ Export Excel - AR Provider โดยจะมีการ Switch Dr/Cr ไว้รอ
และจะมีการตั้ง Debit เข้าธนาคารเอาไว้ให้

โปรดตรวจสอบความถูกต้องก่อนทำการ Import เพื่อสร้าง Journal Entry

.. image:: images/reconcile_ar_provider/3_export_excel.png
    :align: center

2. สร้าง Journal Entry ตาม statement ที่ได้รับแจ้ง
########################################################

1. เตรียม Excel โดยใช้ข้อมูลจาก statement ที่ได้รับมา
2. ที่เมนู Journal Entries สร้างรายการใหม่ ซึ่งจะทำหน้าที่เป็น Payment Entry
3. ที่ JE, คลิกเมนู Action > Import Excel โดยเลือก Template = **AR Provider**
4. ตรวจทานให้เรียบร้อยจึงก่อน Post

.. nextslide::

สร้าง Journal Entry ใหม่เพื่อทำหน้าที่เป็น Payment Entry

.. image:: images/reconcile_ar_provider/4_create_payment_entry.png
    :align: center

.. nextslide::

เพิ่มรายการด้วยการ Import Excel ตามที่ได้เตรียมไว้โดยเลือก Template = **AR Provider**

.. image:: images/reconcile_ar_provider/5_import_excel.png
    :align: center

.. nextslide::

ตรวจสอบให้แน่ใจ แล้วจึงค่อย Post (เฉพาะรายการที่ Posted แล้วเท่านั้นที่ Mass Reconcile จะสนใจ)

.. image:: images/reconcile_ar_provider/6_post_payment_entry.png
    :align: center

3. ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile เลือก Profile = **113101 ลูกหนี้การค้า**
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปให้ทำการ Reverse Entry

End.

Clear AP Commission
-------------------

การบันทึกคู่บญชีอัตโนมัติของ Delivery Complete ได้ทำให้เกิดค่าคอมมิชชั่น ซึ่งทาง 945 ต้องทำจ่ายให้กันผู้ได้รับส่วนแบ่งที่เกี่ยวข้อง

1. เตรียมไฟล์ Excel จากระบบ เพื่อการจ่ายค่า Commission
2. นำเข้ารายการเพื่อสร้าง JE และ Split JE สำหรับแต่ละบุคคล
3. ออก Withholding Tax Cert ให้กับผู้รับเงินพร้อมๆกัน
4. ทำการ Reconcile และตรวจสอบผลลัพธ์

1. เตรียมไฟล์ Excel เพื่อการจ่ายค่า Commission
##################################################

สำหรับ Commission จะดูตามวันที่ (เช่น ศุกร์ถัดไป) โดยสามารถค้นหาที่เมนู Journal Items ด้วย Filter ดังต่อไปนี้

1. กรองรายการด้วย Favorite = **AP Commission**
2. เลือกรายการที่ต้องการ Export
3. คลิกเมนู Action > Export Excel โดยเลือก Template = **AP Commission**

.. image:: images/reconcile_ap_commission/1_search_journal_items.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ap_commission/2_export_excel_wizard.png
    :align: center

.. nextslide::

ตัวอย่าง Excel ของการ Export Excel - AP Commission โดยจะมีการ Switch Dr/Cr ไว้รอ
และจะมีการตั้ง Debit เข้าธนาคารเอาไว้ให้ **แต่ผู้ใช้งานต้องเพิ่มรายการ Withholding Tax ให้กับผู้รับ Commission เอง**

.. image:: images/reconcile_ap_commission/3_export_excel.png
    :align: center

2. นำเข้ารายการเพื่อสร้าง JE และ Split JE สำหรับแต่ละบุคคล
############################################################

1. เตรียม Excel และตรวจทานความถูกต้อง และสร้าง Journal Entries เพื่อทำ Payment Entry
2. ที่ JE, คลิกเมนู Action > Import Excel โดยเลือก Template = **AP Commission**
3. ทำการ Split Journal Entry สำหรับแต่ละผู้รับส่วนแบ่ง

.. nextslide::

สร้าง Journal Entry ใหม่เพื่อทำหน้าที่เป็น Payment Entry

.. image:: images/reconcile_ap_commission/4_create_payment_entry.png
    :align: center

Import Excel ตามที่ได้เตรียมด้วย Template = **AP Commission**

.. image:: images/reconcile_ap_commission/5_import_excel.png
    :align: center

.. nextslide::

ในขณะที่ JE ยังมีสถานะ **Draft** ให้ทำการ Split Journal Entry by Partner, ระบบทำการ Split JE ตาม
จำนวนผู้รับส่วนแบ่ง (และลบ Journal Entry ตั้งต้นนี้)

.. image:: images/reconcile_ap_commission/6_split_je.png
    :align: center

.. nextslide::

ในตอนนี้เราจะมี Journal Entries สำหรับผู้รับส่วนแบ่งทุกๆคน เพื่อเราจะได้สามารถออก WHT Cert. ได้พร้อมๆกัน

.. image:: images/reconcile_ap_commission/7_new_split_je.png
    :align: center

3. ออก Withholding Tax Cert ให้กับผู้รับเงินพร้อมๆกัน
####################################################

จาก Journal Entries ทั้งหมดในขั้นตอนก่อน เลือก Action > Create Withholding Cert

.. image:: images/reconcile_ap_commission/8_create_wht_certs.png
    :align: center

.. nextslide::

ระบบจะช่วยสร้าง Cert จากรายการที่บันทึก Account Code - WHT

.. image:: images/reconcile_ap_commission/9_create_wht_certs_wizard.png
    :align: center

.. nextslide::

ระบบจะสร้าง Withholding Tax Certs ให้ตามจำนวนของ Journal Entry

.. image:: images/reconcile_ap_commission/10_wht_cert.png
    :align: center

ให้ผู้ใช้งานตรวจสอบความถูกต้องแล้วกดปุ่ม Done

.. nextslide::

เลือก Print > Withholding Tax Cert เป็น PDF (สามารถ print ได้พร้อมๆกันหลายรายการ)

.. image:: images/reconcile_ap_commission/11_print_wht_cert.png
    :align: center

.. note::
    ผู้ใช้งานสามารถดู Certificate. ทั้งหมดในภายหลังได้ที่เมนู Accounting > Vendors > WT Certificates

4. ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile เลือก Profile = **212101 เจ้าหนี้การค้า**
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปให้ทำการ Reverse Entry

End.

Clear AP Transport
--------------------

#. ย้ายเจ้าหนี้ประมาณการค่าขนส่งเป็นเจ้าหนี้ ด้วย Excel
    #. เลือกรายการที่ต้องทำจ่าย (ครบกำหนดวันที่ 15 ของ 2 เดือนหลัง) และ Export Excel ตั้งต้น
    #. ปรับปรุง JE และ Import Excel เพื่อสร้าง JE ล้างประมาณการเข้าเจ้าหนี้
    #. ทำการ Reconcile และตรวจสอบผลลัพธ์
#. การเคลียร์เจ้าหนี้ โดยจะมีการหักภาษี ณ ที่จ่าย ภงด 53 ไว้ 1% ด้วย Journal Entry
    #. สร้าง Journal Entry ด้วย Template ที่เตรียมไว้ล่วงหน้า
    #. Manual Reconcile เพื่อเคลียร์เจ้าหนี้ขนส่ง
    #. ออกเอกสาร Withholding Tax Cert.

1.1 เลือกรายการที่ต้องทำจ่าย
############################################

รายการประมาณการค่าขนส่ง จะดูตามวันที่ 15 โดยสามารถค้นหาที่เมนู Journal Items

1. กรองรายการด้วย Favorite = **AP Transport**
2. เลือกรายการที่ต้องการ Export
3. คลิกเมนู Action > Export Excel โดยเลือก Template = **AP Transport Estimated**

.. image:: images/reconcile_ap_transport/1_search_journal_items.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ap_transport/2_export_excel_wizard.png
    :align: center

.. nextslide::

ตัวอย่าง Excel ของการ Export Excel - AP Transport Estimated โดยจะมีการ Switch Dr/Cr ไว้รอ
**แต่ผู้ใช้งานต้องเพิ่มรายการปรับปรุงรายการเมื่อเทียบกับข้อมูล External เอง**

.. image:: images/reconcile_ap_transport/3_export_excel.png
    :align: center

.. nextslide::

1. ข้อมูลที่ Export ออกมาจาก Odoo
2. ให้นำข้อมูลที่ได้จาก Transporter ใส่ที่ External
3. ระบบจะเปรียบเทียบรายการที่ตรงกัน
4. ให้ทำการลบรายการที่ไม่ตรงกัน
5. ระบบได้ทำการคำนวนส่วนต่างค่าขนส่งให้เบื้องต้น แต่ผู้ใช้งานสามารถใส่ส่วนลดและอื่นๆได้เอง

1.2 Import Excel เพื่อสร้าง JE ล้างประมาณการเข้าเจ้าหนี้
############################################################

สร้าง Journal Entry **Vendor Bill** ใหม่ แล้วจึงสร้างรายการด้วยการ Import Excel,
ทำการกำหนดวันที่ถึงกำหนดขำระ และใส่ Remarks เพื่อการอ้างถึงในภายหลัง

.. image:: images/reconcile_ap_transport/4_review_and_post.png
    :align: center

.. note::
    ต้องตรวจสอบให้แน่ใจ แล้วจึงค่อย Post

1.3 ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile เลือก Profile = **212107 ประมาณการ - เจ้าหนี้การค้า**
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปให้ทำการ Reverse Entry

2.1 สร้าง Journal Entry ด้วย Template - Pay for AP Transporter
##################################################################

**Menu:** Accounting > Accounting > Create Entry from Template

ให้เลือก Template ให้ถูกต้องและกดปุ่ม Next

.. image:: images/reconcile_ap_transport/5_create_je_ap_transporter.png
    :align: center

.. nextslide::

ใส่ข้อมูลตามรูป แล้วกดปุ่ม Create Journal Entry

.. image:: images/reconcile_ap_transport/6_create_je_ap_transporter.png
    :align: center

.. nextslide::

ระบบจะช่วยสร้าง Journal Entry ให้ พร้อมๆกับช่วยคำนวน WHT 1% ด้วย

.. image:: images/reconcile_ap_transport/7_je_ap_transporter.png
    :align: center

2.2 Manual Reconcile ลูกหนี้ค่าขนส่ง
#########################################

ขึ้นตอนนี้เราจะใช้วิธี Manual Reconcile โดยไม่ผ่าน Mass Automatic Reconcile เหมือนกรณีอื่นๆเนื่องจาก
จะเป็นการล้าง รายการเจ้าหนี้ Transporter ตรงๆ (ไม่มี Parcel ID)

1. ไปที่หน้าต่าง Journal Items
2. Filter รายการ Unreconciled, เจ้าหนี้การค้า ที่ไม่มี Parcal ID
3. เลือกรายการ และทำการ Reconcile ผ่าน Action
4. ตรวจสอบ และกดปุ่ม Reconcile

.. nextslide::

.. image:: images/reconcile_ap_transport/8_manual_reconcile.png
    :align: center

2.3 ออก Withholding Tax Cert ให้กับผู้รับเงิน
################################################

1. จาก Journal Entry ในขั้นตอนก่อน เลือก Action > Create Withholding Cert
2. ระบบจะช่วยสร้าง Cert จากรายการที่บันทึก Account Code - WHT
3. ให้ผู้ใช้งานกรอกข้อมูลให้ครบแล้วกด Save ตรวจสอบความถูกต้องแล้วกดปุ่ม Done
4. เลือก Print > Withholding Tax Cert เป็น PDF

End.

Clear AP Service
--------------------

การบันทึกคู่บญชีอัตโนมัติของ Delivery Complete ได้ทำให้เกิดค่าบริการ ซึ่งทาง 945 ต้องทำจ่ายให้กับผู้ให้บริการ

1. เลือกรายการที่ต้องทำจ่าย โดยดูตามวันที่ (ครบกำหนดวันที่ 15 ของ 2 เดือนหลัง) และนำไปสร้าง Journal Entry สำหรับการจ่ายเงิน
2. ออก Withholding Tax Cert ให้กับผู้รับเงิน
3. บันทึกเลขที่ Tax Invoice และทดสอบการออกรายงานภาษี
4. ทำการ Reconcile และตรวจสอบผลลัพธ์

1. เลือกรายการที่ต้องทำจ่าย
################################

รายการประมาณการค่าขนส่ง จะดูตามวันที่ 15 โดยสามารถค้นหาที่เมนู Journal Items

1. กรองรายการด้วย Favorite = **AP Service**
2. เลือกรายการที่ต้องการ Export
3. คลิกเมนู Action > Export Excel โดยเลือก Template = **AP Service**

.. image:: images/reconcile_ap_service/1_search_journal_items.png
    :align: center

.. nextslide::

.. image:: images/reconcile_ap_service/2_export_excel_wizard.png
    :align: center

.. nextslide::

ตัวอย่าง Excel ของการ Export Excel - AP Service โดยจะมีการ Switch Dr/Cr ไว้รอ
**แต่ผู้ใช้งานต้องเพิ่มรายการปรับปรุงรายการเมื่อเทียบกับข้อมูล External เอง**

.. image:: images/reconcile_ap_transport/3_export_excel.png
    :align: center

.. nextslide::

1. ข้อมูลที่ Export ออกมาจาก Odoo
2. ให้นำข้อมูลที่ได้จาก Transporter ใส่ที่ External
3. ระบบจะเปรียบเทียบรายการที่ตรงกัน
4. ให้ทำการลบรายการที่ไม่ตรงกัน
5. ระบบได้ทำการคำนวนส่วนต่างค่าขนส่งให้เบื้องต้น แต่ผู้ใช้งานสามารถใส่ส่วนลดและอื่นๆได้เอง

2. ออก Withholding Tax Cert ให้กับผู้รับเงิน
################################################

1. จาก Journal Entry ในขั้นตอนก่อน เลือก Action > Create Withholding Cert
2. ระบบจะช่วยสร้าง Cert จากรายการที่บันทึก Account Code - WHT
3. ให้ผู้ใช้งานกรอกข้อมูลให้ครบแล้วกด Save ตรวจสอบความถูกต้องแล้วกดปุ่ม Done
4. เลือก Print > Withholding Tax Cert เป็น PDF

3. Import Excel เพื่อสร้าง JE สำหรับการจ่าย
################################################

สร้าง Journal Entry **Bank** ใหม่ แล้วจึงสร้างรายการด้วยการ Import Excel

.. image:: images/reconcile_ap_service/4_review_and_post.png
    :align: center

.. nextslide::

1. เนื่องจากมีรายการ VAT ที่ต้องการ Tax Invoice ระบบจะยังไม่ให้ Post
2. ใส่ข้อมูล Tax Invoice
3. ทำการ Post

4 ทำการ Reconcile และตรวจสอบผลลัพธ์
############################################

1. ที่เมนู Mass Automatic Reconcile เลือก Profile = **212101 เจ้าหนี้การค้า**
2. กดปุ่ม Start Auto Reconciliation ระบบจะทำการ Reconcile รายการที่มี Partner และ Parcel ID เดียวกัน
3. กดปุ่ม Display Items Reconciled On The Last Run เพื่อดูรายการที่ถูก Reconciled ไป
4. หากต้องการยกเลิกสิ่งที่ทำไปให้ทำการ Reverse Entry

End.
