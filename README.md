# Customer-Profiles
<query>
        <title>Customer Profiles</title>
        <permissions>MARKETING</permissions>
        <sql><![CDATA[
            SELECT ce.entity_id AS 'customer_id', ce.email, DATE_FORMAT(ce.created_at, '%m/%d/%Y') AS 'sign_up_date', GREATEST(SUM(soi.qty_ordered), 
                SUM(soi.qty_shipped), SUM(soi.qty_invoiced)) - SUM(soi.qty_canceled) - SUM(soi.qty_refunded) - SUM(soi.qty_returned) AS 'total_actual_qty', COUNT(DISTINCT soi.order_id) AS 'number_of_orders', 
                FORMAT(SUM(soi.price * (GREATEST(soi.qty_ordered, soi.qty_shipped, soi.qty_invoiced) - soi.qty_canceled - soi.qty_refunded - soi.qty_returned)), 'C0') AS 'total_revenue',  
                FORMAT((SUM(soi.price * (GREATEST(soi.qty_ordered, soi.qty_shipped, soi.qty_invoiced) - soi.qty_canceled - soi.qty_refunded - soi.qty_returned)))/(COUNT(DISTINCT soi.order_id)), 'C') AS 'AOV', 
                DATE_FORMAT(so.created_at, '%m/%d/%Y') AS 'last_order', cae.city, cae.region, cae.postcode, DATE_FORMAT(ce.dob, '%m/%d/%Y') AS 'birthday'
            FROM sales_order so JOIN sales_order_item soi ON so.entity_id = soi.order_id JOIN customer_entity ce ON ce.entity_id = so.customer_id JOIN customer_address_entity cae ON cae.entity_id = ce.default_shipping 
            WHERE soi.store_id = ? AND DATE(so.created_at) >= ? AND DATE(soi.created_at) <= ?
            GROUP BY ce.entity_id;]]></sql>
        <field id="0" type="String">
            <label>Store/Country ID</label>
            <preset value="1">USA Store View</preset>
            <preset value="2">UK Store View</preset>
            <preset value="3">France Store View</preset>
            <preset value="4">Netherlands Store View</preset>
            <preset value="5">Germany Store View</preset>
            <preset value="6">Belgium - Dutch Store View</preset>
            <preset value="7">Belgium - French Store View</preset>
        </field>
        <field id="1" type="date">
            <label>From</label>
            <dateformat/>
        </field>
        <field id="2" type="date">
            <label>To</label>
            <dateformat/>
        </field>
        <filename>Customer Profiles<string id="0"/><date id="1"/> and <date id="2"/></filename>
    </query>
