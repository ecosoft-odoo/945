> Accounting Report
=====================
รายงานทางการเงินสำหรับ Odoo V.13 (Enterprise Edition)


รายงานทางการเงิน
-----------------

    รายงานทางการเงิน หรือ การสื่อข้อมูลทางการเงินเพื่อให้บุคคลภายใน และบุคคลภายนอกนําไปใช้ในการตัดสินใจเชิงเศรษฐกิจ
    โดยมีงบการเงินเป็นส่วนประกอบที่สําคัญของการรายงานทางการเงิน ทั้งนี้อาจจะทําในรูปแบบของรายงานภายในกิจการ รายงานประจําปี

Odoo มีรายงานทางการเงินดังนี้

.. image:: images/accounting_report/all_report.png
   :align: center

1. Trial Balance : งบทดลอง
2. General Ledger : รายงานบัญชีแยกประเภท
3. Profit and Loss : งบกำไรขาดทุน
4. Balance Sheet : งบดุล
5. Executive Summary : รายงานสัดส่วนทางการเงิน
6. Cash Flow Statement : งบกระแสเงินสด
7. Partner Ledger : รายละเอียดลูกหนี้/เจ้าหนี้
8. Aged Receivable : วิเคราะห์อายุลูกหนี้
9. Aged Payable : วิเคราะห์อายุเจ้าหนี้


วิธีการกรองรายงานทางการเงิน
----------------------------

เมื่อเข้ามาที่หน้ารายงาน ระบบจะแสดงข้อมูลเริ่มต้น ดังนี้

.. |date| image:: images/accounting_report/date.png

1. |date| โดยระบบจะแสดง **ค่าเริ่มต้นเป็นเดือนปัจจุบัน** แต่สามารถเลือกได้ว่าต้องการดูรายงาน ในช่วงเวลาใด

    * This Month : เดือนปัจจุบัน
    * This Quarter : ไตรมาสปัจจุบัน
    * This Financial Year : ปีปัจจุบัน
    * Last Month : เดือนก่อนหน้า
    * Last Quarter : ไตรมาสก่อนหน้า
    * Last Financial Year : ปีก่อนหน้า
    * Custom : สามารถเลือกวันเริ่มต้น (Start Date) และ วันสิ้นสุด (End Date)

.. |comp| image:: images/accounting_report/compare.png

2. |comp| สามารถดูแบบเปรียบเทียบกันได้

    * No Comparison : ไม่มีการเปรียบเทียบ
    * Previous Period : เดือนก่อนหน้า
    * Same Period Last Year : ปีก่อนหน้า
    * Custom : สามารถเลือกวันที่เริ่มต้น (Start Date) และ วันสิ้นสุด (End Date) เพื่อมาเปรียบเทียบได้

.. |jour| image:: images/accounting_report/journal.png

3. |jour| สามารถกำหนดได้ว่า จะแสดง Journal ใดบ้าง

.. |opt| image:: images/accounting_report/option.png

4. |opt| สามารถเลือกได้ว่าต้องการให้ระบบแสดงเอกสารที่อยู่ในสถานะใดบ้าง
โดยระบบจะเลือกเอกสาร Stage : Posted Entries Only เป็นค่าเริ่มต้น

    * Include Unposted Entries : แสดงเอกสารรวมเอกสารที่ยังไม่ถูก Posted
    * Hierarchy and Subtotals : แสดงหน้าวิวเป็นรูปแบบ Hierarchy
    * Unfold All : แสดงทั้งหมด

.. |acc| image:: images/accounting_report/acc.png

5. |acc| สามารถเลือกได้ว่าจะดู Account ประเภทไหน โดยค่าเริ่มต้นระบบจะเลือกทั้งหมด "Both"

    * Payable เจ้าหนี้
    * Receivable ลูกหนี้

.. |part| image:: images/accounting_report/partner.png

6. |part| สามารถค้นหาเจ้าหนี้/ลูกหนี้ที่ต้องการได้ โดยค่าเริ่มต้นระบบจะแสดงทั้งหมด


วิธี Export รายงาน
------------------

.. image:: images/accounting_report/export1.png
   :align: center

*  Print Preview : Export รายงานออกมาเป็นไฟล์ PDF

*  Export (XLSX) : Export รายงานออกมาเป็นไฟล์ XLSX


รายงานทางการเงินในระบบ Odoo
-------------------------------

1. Trial Balance  งบทดลอง
##############################

    งบทดลอง เป็นงบที่จัดทำขึ้นด้วยการนำยอดดุลในบัญชีแยกประเภทต่าง ๆ (5 หมวดบัญชี) ไม่ว่าจะอยู่ด้านเดบิต หรือเครดิตก็ตาม มาคำนวณหายอดคงเหลือทั้งสองด้าน
    เพื่อพิสูจน์ความถูกต้องในการบันทึกบัญชีตามระบบบัญชีคู่ ณ วันใดวันหนึ่ง เช่น ทุกสิ้นเดือน ทุก 3 เดือน หรือเมื่อสิ้นงวดบัญชี


.. image:: images/accounting_report/trial_balance.gif
   :align: center


2. General Ledger รายงานบัญชีแยกประเภท
#########################################

    รายงานบัญชีแยกประเภท รายงานที่แสดงข้อมูลตามรหัสบัญชีตามช่วงเวลาที่กำหนด


.. image:: images/accounting_report/general_ledger.gif
   :align: center


3. Profit and Loss งบกำไรขาดทุน
################################

    งบกําไรขาดทุน เป็นรายงานทางการเงินที่แสดงผลการดําเนินงานของกิจการ โดยนํารายได้และค่าใช้จ่ายที่เกิดขึ้นในงวดบัญชีนั้น ๆ
    มาเปรียบเทียบกัน ผลต่างที่เกิดขึ้นจะเป็นกําไรหรือขาดทุนสุทธิ


.. image:: images/accounting_report/profit_loss.gif
   :align: center


4. Balance Sheet งบดุล
###########################

    งบดุล (Balance Sheet) เป็นรายงานที่แสดงให้เห็นถึงฐานะการเงินของกิจการ ณ วันใดวันหนึ่ง ว่ากิจการมีสินทรัพย์ หนี้สิน ทั้งสิ้นเป็นจํานวนเท่าใด
    ประกอบด้วยรายการใดบ้าง และคงเหลือเป็นสินทรัพย์สุทธิหรือส่วนของเจ้าของกิจการเป็นจํานวนทั้งสิ้นเท่าใด


.. image:: images/accounting_report/balance_sheet.gif
   :align: center


5. Executive Summary รายงานสัดส่วนทางการเงิน
############################################

    เป็นรายงานที่แสดงภาพรวมทางการเงิน เป็นบทสรุปสำหรับการบหริหารภายใน

.. image:: images/accounting_report/executive_summary.gif
   :align: center



6. Cash Flow Statement  งบกระแสเงินสด
############################################

    งบกระแสเงินสด คือ รายงานแสดงถึงกิจกรรมที่เกิดขึ้นในกิจการ ประกอบด้วย กิจกรรมการดำเนินงาน กิจกรรมการลงทุน และกิจกรรมจัดหาเงิน


.. image:: images/accounting_report/cash_flow.gif
   :align: center


7. Partner Ledger รายละเอียดลูกหนี้/เจ้าหนี้
############################################


.. image:: images/accounting_report/partner_ledger.gif
   :align: center



8. Aged Receivable วิเคราะห์อายุลูกหนี้
########################################


.. image:: images/accounting_report/aged_receivable.gif
   :align: center



9. Aged Payable วิเคาะห์อายุเจ้าหนี้
####################################


.. image:: images/accounting_report/aged_payable.gif
   :align: center





