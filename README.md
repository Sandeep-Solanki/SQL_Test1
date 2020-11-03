# SQL_Test1

-- Note desc for Completion and Corrective Action Comments --

With T1_CC as
(select distinct SRVREQ_WID ,  
listagg(to_char(CREATED_DT,'DD-MON-YYYY HH:MI:SS AM') ||' - '|| Note_Desc, '; ' ) within group (order by CREATED_DT desc ) as Completion_Comments
from WC_SRVREQ_NOTES_D
where SRVREQ_WID  in ('81250439','81250293','81250298')
and Note_type = 'Completion Comments'
group by SRVREQ_WID
) ,

T2_CAC as
(select distinct SRVREQ_WID , 
listagg(to_char(CREATED_DT,'DD-MON-YYYY HH:MI:SS AM') ||' - '|| Note_Desc, '; ' ) within group (order by CREATED_DT desc ) as Corrective_Action_Comments
from WC_SRVREQ_NOTES_D
where SRVREQ_WID  in ('81250439','81250293','81250298')
and Note_type = 'Corrective Action Comments'
group by SRVREQ_WID)

Select  
COALESCE(T1_CC.SRVREQ_WID,T2_CAC.SRVREQ_WID) SRVREQ_WID , 
T1_CC.Completion_Comments, 
T2_CAC.Corrective_Action_Comments
From T1_CC 
Full Outer Join T2_CAC on T1_CC.SRVREQ_WID = T2_CAC.SRVREQ_WID
