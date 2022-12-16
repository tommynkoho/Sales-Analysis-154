# Sales-Analysis-154

select A.unique_identifier,   A.STR_NBR,  A.SKU_NBR,  A.UNT_SLS,  SLS_AMT as Sales_in_2021,  A.SLS_DT,   A.FSCL_MTH_NM, B.aisle, B.Bay

from (SELECT concat(STR_NBR,"-",SKU_NBR,"-", FSCL_MTH_NM) as unique_identifier, STR_NBR, SKU_NBR,UNT_SLS, SLS_AMT, SLS_DT, FSCL_MTH_NM


 FROM `pr-edw-views-thd.SLS.SKU_STR_DAY_MVNDR_SLS` a

JOIN `pr-edw-views-thd.SHARED.CAL_PRD_HIER_FD` b ON a.SLS_DT=b.CAL_DT

---JOIN `pr-edw-views-thd.SHARED.SKU_HIER_FD -----SKU ATTRIBUTES SOURCE ie to get SKU DESCRIPTION, DEPT, CLASS

---JOIN `pr-edw-views-thd.SHARED.STR_HIER_FD -----STORE ATTRIBUTES SOURCE ie to get store name, location, district, region and division.


 where SLS_DT between '2021-02-01' and '2021-08-30' and STR_NBR ='0154'
 
 group by Str_NBR, SKU_NBR, UNT_SLS, SLS_AMT, SLS_DT, FSCL_MTH_NM) as A

 join

 (select subquery1.SKU_NUMBER, subquery1.store_number, concat(subquery1.unique_identifier,"-",FSCL_MTH_NM) as unique_identifier,subquery1.New_Date,subquery1.AISLE,subquery1.BAY,subquery1.sources, c.FSCL_MTH_NM,

from (SELECT A.SKU_NUMBER, B.STORE_NUMBER as store_number,Concat(B.STORE_NUMBER,"-",A.SKU_NUMBER) AS unique_identifier,Cast(A.CREATE_TIMESTAMP as Date) as New_Date, A.AISLE, A.BAY,B.SOURCE as sources

FROM `pr-store-merch-thd.STORE_BAY_INTEGRITY.SHELF_OUT` as A

Join `pr-store-merch-thd.STORE_BAY_INTEGRITY.SHELF_OUT_SESSION`  as B ON

   A.SHELF_OUT_SESSION_ID= B.SHELF_OUT_SESSION_ID

where source= "ZIPPEDI_AISLE_BY_AISLE" or source= "ZIPPEDI"

order by A.SKU_NUMBER, B.store_number, unique_identifier,New_Date,A.AISLE,A.BAY,sources) as subquery1

join `pr-edw-views-thd.SHARED.CAL_PRD_HIER_FD` C ON subquery1.New_Date = C.CAL_DT

WHERE subquery1.New_Date between '2022-02-01' and '2022-09-01' and subquery1.store_number= '0154') as B on

A.unique_identifier = B.unique_identifier;

![image](https://user-images.githubusercontent.com/17092274/208140452-e5352264-087c-4651-881e-e0ae6c6c5b1a.png)
