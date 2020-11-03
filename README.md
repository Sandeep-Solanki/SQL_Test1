# SQL_Test1

-- Note desc for Completion and Corrective Action Comments --
-- Test comment to branch B1 
-- Developer 2 : Changing the dilimiter 

With T1_CC as
(select distinct SRVREQ_WID ,  

listagg(to_char(CREATED_DT,'DD-MON-YYYY HH:MI:SS AM') ||' >> '|| Note_Desc, '; ' ) within group (order by CREATED_DT desc ) as Completion_Comments
from WC_SRVREQ_NOTES_D
where SRVREQ_WID  in ('81250439')
and Note_type = 'Completion Comments'
group by SRVREQ_WID
) ,

T2_CAC as
(select distinct SRVREQ_WID , 

listagg(to_char(CREATED_DT,'DD-MON-YYYY HH:MI:SS AM') ||' >> '|| Note_Desc, '; ' ) within group (order by CREATED_DT desc ) as Corrective_Action_Comments
from WC_SRVREQ_NOTES_D
where SRVREQ_WID  in ('81250439')
and Note_type = 'Corrective Action Comments'
group by SRVREQ_WID)

-- New code added on 3rd Nov
-- Start
T3_CA as
(select distinct SRVREQ_WID , 
Listagg(to_char(CREATED_DT,'DD-MON-YYYY HH:MI:SS AM') ||' - '|| Note_Desc, '; ' ) within group (order by CREATED_DT desc ) as Containment_Action
from WC_SRVREQ_NOTES_D
where SRVREQ_WID  in ('81250439','81250293','81250298')
and Note_type = 'Containment Action'
group by SRVREQ_WID)

-- End 

Select  
COALESCE(T1_CC.SRVREQ_WID,T2_CAC.SRVREQ_WID,T3_CA.SRVREQ_WID) SRVREQ_WID , 
T1_CC.Completion_Comments, 
T2_CAC.Corrective_Action_Comments, 
T3_CA.Containment_Action
From T1_CC 
Full Outer Join T2_CAC on T1_CC.SRVREQ_WID = T2_CAC.SRVREQ_WID
Full Outer Join T3_CA on T2_CAC.SRVREQ_WID = T3_CA.SRVREQ_WID
