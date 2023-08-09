# CAMHSQL
CAMH SQL Check

_**Primarily concerning the Access dataset:**_

1. In contrast to Dundas BI, there are distinctions in the Transfer In and Transfer Out aspects.
Within Tableau Prep and Tableau, we handled Transfer To Institution (TTI) and Transfer To Other (TTO) as distinct entities.

2. there exist minor disparities in the **Discharge** and **ADMN** data when compared with Dundas BI.

3. The LongStay Calculation necessitates a re-evaluation, first with Keyrus and subsequently with CAMH.



_**<Long IPLOSDays 2>**_

IF ISNULL ([Discharge Date])  THEN
    ROUND(DATEDIFF('minute', [Date], TODAY()) / (60*24), 1)
ELSE
    NULL
END
> Based on the code, if the "Discharge Date" is null then we need to calculate how many days this MRN fixed data was in charge in hospital.

**<Long_LongStayTF>**

{ FIXED [MRN] : MAX(IIF (isnull([Discharge Date])  AND [Long IPLOS Days_2] >= 365, 1, 0)) }

>So, in plain terms, the code calculates whether, for each patient identified by their [MRN], 
there's a case where the patient's [Discharge Date] is null (they are still admitted) and their length of stay ([Long IPLOS Days_2]) is at least 365 days. 

>The final result for each patient's [MRN] is 1 if this condition is met for any instance, and 0 otherwise. This could be used, for example, to flag patients who have been in the hospital for at least a year and are still not discharged. So after we count of the each MRN which has a value of 1, it is the answer of Longstay


        
**Dundas BI**
<img width="735" alt="DD_Inpatient Dashboard_ACCESS+ 22_09_AcuteCare" src="https://github.com/HarimJung/CAMHSQL/assets/48680125/ca850970-f322-44df-b6d6-c08faef73180">

**Tableau IPD**
![Inpatient Dashboard_ACCESS+ 22_09_AcuteCare](https://github.com/HarimJung/CAMHSQL/assets/48680125/a75d683e-4cc4-42b1-87c0-8804038507c4)

Some case, numbers are matched, though some cases, it is unmatched. case of "Sep 2022" with "Acute Care" location filtered dashboard,
Mainly Admission has wrong number though Transfer In and transfer Out, Discharges are fine.
Interestingly, ACL Rate is working fine even with "Patient Days, ALC dys" inside of Hover text.

_So, overall, I may guess it is highly depending on the updated dataset environment._
