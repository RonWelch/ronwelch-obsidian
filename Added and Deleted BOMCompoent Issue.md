---
categories:
  - "[[Projects]]"
type: []
org: []
start:
year:
url:
status:
tags:
  - DTaaS
---


MCN: 510483

87T0203-FRP4-UE A.6 current Release: 


42     12594171-UE - DeletedBomComponent
42     12594171-UE - AddedBomComponent

currentPart: 87T0203-FRP4-UE, CARRIER, COMMAND POST, 19207, A.6 (York)
previousPart: 87T0203-FRP4-UE, CARRIER, COMMAND POST, 19207, -.1 (York)

<AddedBOMComponents NAME="AddedBOMComponents" STATUS="0" TYPE="Unknown">
<BOMComponent>
    <ObjectID>OR:wt.part.WTPartUsageLink:20854106628:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</ObjectID>
    <Class>com.ptc.windchill.esi.BOMComponent</Class>
    <PartNumber>12594171-UE</PartNumber>
    <AssemblyPartNumber>87T0203-FRP4-UE</AssemblyPartNumber>
    <AssemblyPartID>OR:wt.part.WTPart:20854105755:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</AssemblyPartID>
    <Quantity>1</Quantity>
    <Unit>ea</Unit>
    <LineNumber>42</LineNumber>
    <FindNumber/>
    <AlternateItemGroup/>
    <ComponentId>cb7942ac-b673-45d2-a091-302beed12c17</ComponentId>
    <IsChildPhantom>false</IsChildPhantom>
    <IsPartConfigurable/>
    <IsPartCollapsible/>
    <AssociatedEffectivityID>OR:wt.part.ProductSerialNumberEffectivity:20471916082:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</AssociatedEffectivityID>
    <IsRawMaterial>false</IsRawMaterial>
    <OSN>1</OSN>
    <TargetID>130</TargetID>
</BOMComponent>




<DeletedBOMComponents NAME="DeletedBOMComponents" STATUS="0" TYPE="Unknown">

<BOMComponent>
    <ObjectID>OR:wt.part.WTPartUsageLink:20471769879:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</ObjectID>
    <Class>com.ptc.windchill.esi.BOMComponent</Class>
    <PartNumber>12594171-UE</PartNumber>
    <AssemblyPartNumber>87T0203-FRP4-UE</AssemblyPartNumber>
    <AssemblyPartID>OR:wt.part.WTPart:20854105755:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</AssemblyPartID>
    <Quantity>1</Quantity>
    <Unit>ea</Unit>
    <LineNumber>42</LineNumber>
    <FindNumber/>
    <AlternateItemGroup/>
    <ComponentId>370b2153-afcc-4c08-bdee-824e33e7514e</ComponentId>
    <IsChildPhantom>false</IsChildPhantom>
    <IsPartConfigurable/>
    <IsPartCollapsible/>
    <AssociatedEffectivityID>OR:wt.part.ProductSerialNumberEffectivity:20471916082:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</AssociatedEffectivityID>
    <IsRawMaterial>false</IsRawMaterial>
    <OSN>1</OSN>
    <TargetID>130</TargetID>
</BOMComponent>







<ChangedParts NAME="ChangedParts" STATUS="0" TYPE="Unknown">

<Effectivity>
    <ObjectID>OR:wt.part.ProductSerialNumberEffectivity:20471916082:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</ObjectID>
    <Class>com.ptc.windchill.esi.Part</Class>
    <StartEffectivity>AMPV00458</StartEffectivity>
    <EndEffectivity/>
    <EffectivityCxtPartNumber>87T0203-FRP4-UE</EffectivityCxtPartNumber>
    <EffectivityType>End Item Serial Number Effectivity</EffectivityType>
    <EffectivityQualifier>Exact</EffectivityQualifier>
    <PartMaster>12594171-UE</PartMaster>
    <TargetID>130</TargetID>
</Effectivity>
<Part>
    <ObjectID>OR:wt.part.WTPart:20412677105:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</ObjectID>
    <Class>com.ptc.windchill.esi.Part</Class>
    <LastChangedBy>sa_john_evans20</LastChangedBy>
    <Number>12594171-UE</Number>
    <DefaultUnit>ea</DefaultUnit>
    <Name>SINCGARS AN/VRC-92F INSTALLATION</Name>
    <PartType>separable</PartType>
    <Source>make</Source>
    <State>RELEASEDFORPRODUCTION</State>
    <View>York</View>
    <IsPhantom>false</IsPhantom>
    <Version>-</Version>
    <Iteration>4</Iteration>
    <PreviousVersion>-</PreviousVersion>
    <IsConfigurable>standard</IsConfigurable>
    <IsCollapsible>false</IsCollapsible>
    <AssociatedEffectivityID>OR:wt.part.ProductSerialNumberEffectivity:20471916082:774860221-1276518425941-25990108-83-40-65-10@epdmtst2.lna.us.baesystems.com</AssociatedEffectivityID>
    <OracleTemplate>YRK HARD MAKE</OracleTemplate>
    <BasePartNumber>None</BasePartNumber>
    <RVSCWBSElement/>
    <TargetID>130</TargetID>
</Part>






<Option cardinality="duplicate" requestor="wt.part.WTPart" selector="ESIBOMDiffDelegate" serviceClass="com.ptc.windchill.esi.delegate.ESIBOMDifferenceMangementDelegateImpl"/>

./codebase/com/ptc/windchill/esi/conf/esi.service.properties.xconf





wt.services/svc/default/com.ptc.windchill.esi.delegate.ESIBOMDifferenceMangementDelegate/ESIBOMDiffDelegate/wt.part.WTPart/0=com.ptc.windchill.esi.delegate.ESIBOMDifferenceMangementDelegateImpl/duplicate

./codebase/service.properties


com.baesystems.esi.render.bom.BAEESIBOMDifferenceManagementDelegateImpl



retrieveBomDifferencesUsingObjComparison
adjustDelta
adjustUsage (*)


com.baesystems.esi.render.bom.BAEESIObjectComparisonUtility


Levitek does not appear have loaded Component Ids. A change was made to update the OSNs and or Ref deses. The OSN or ref des change without a componentId appears to have caused the add / delete behavior.




12594171-UE
ChangedPart
OSN is unchanged?? NO? ( 1 )
Ref Des is
LIneNumber change from 42 to blank


13096099-GFM is also added and deleted
Unchanged Part
OSN is unchanged ( 1  )
Ref Des is changed
LineNumber change from 9 to none


12639529-UE
Changed Part
Deleted BOMComponent (OSN: 6609)
Added BOMComponent (OSN: 1)
LineNumber change from 17 to nothing



https://www.ptc.com/en/support/article/CS320593?source=search



P and S Stats:

Analysis for .xml messages in: PROD/U__ENTRY/
-----------------------------------
Total Messages:     335
Largest Message:    43.85 MB (ESI-TXN-125-EntryMessage.xml)
Smallest Message:   730 B (ESI-TXN-Blorf-PublicationResultMessage-0000000000000.xml)
Average Size:       3.52 MB
Messages > 10 MB:   54
Newest Message:     2026-06-27 09:54:44 (ESI-TXN-8-PublicationResultMessage-d3896ee3-6157-4bea-8c7d-c906083b0dbc.xml)
Oldest Message:     2026-02-12 21:17:50 (ESI-TXN-8-PublicationResultMessage-5ae079cb-eb93-41d3-9d80-dc47a015b5df.xml)




USC Stats:

nhnaunxlpdmp520:ResponseFiles wcadminusc $ ./sizes.sh
Analysis for directory: .
-----------------------------------
Total Files:       9453
Largest File:      14.61 MB
Smallest File:     2.41 KB
Average Size:      179.41 KB
Files > 10 MB:     11
Newest File:       2026-07-06 14:31:11
Oldest File:       2025-05-27 12:42:21


IBM webMethods B2B supports payloads up to 500 MB

https://www.ibm.com/docs/en/wm-b2b?topic=overview-large-document-handling



The underlying webMethods Universal Messaging platform 





Can I get some stats on  
-Number of messages sent per month Mar-July  

Analysis for .xml messages in: Feb/
-----------------------------------
Total Messages:     46
Largest Message:    48.60 KB (ESI-TXN-15-EntryMessage.xml)
Smallest Message:   24.21 KB (ESI-TXN-12-EntryMessage.xml)
Average Size:       35.92 KB
Messages > 10 MB:   0
Newest Message:     2026-02-19 07:12:30 (ESI-TXN-59-EntryMessage.xml)
Oldest Message:     2026-02-13 10:14:08 (ESI-TXN-11-EntryMessage.xml)


Analysis for .xml messages in: Mar/
-----------------------------------
Total Messages:     25
Largest Message:    24.27 KB (ESI-TXN-60-EntryMessage.xml)
Smallest Message:   24.27 KB (ESI-TXN-60-EntryMessage.xml)
Average Size:       24.27 KB
Messages > 10 MB:   0
Newest Message:     2026-03-25 11:56:39 (ESI-TXN-87-EntryMessage.xml)
Oldest Message:     2026-03-05 08:25:49 (ESI-TXN-61-EntryMessage.xml)


Analysis for .xml messages in: Apr/
-----------------------------------
Total Messages:     20
Largest Message:    14.36 MB (ESI-TXN-105-EntryMessage.xml)
Smallest Message:   24.26 KB (ESI-TXN-88-EntryMessage.xml)
Average Size:       3.63 MB
Messages > 10 MB:   2
Newest Message:     2026-04-17 07:49:20 (ESI-TXN-110-EntryMessage.xml)
Oldest Message:     2026-04-03 19:37:49 (ESI-TXN-88-EntryMessage.xml)

Analysis for .xml messages in: May/
-----------------------------------
Total Messages:     48
Largest Message:    43.85 MB (ESI-TXN-125-EntryMessage.xml)
Smallest Message:   107.09 KB (ESI-TXN-163-EntryMessage.xml)
Average Size:       10.85 MB
Messages > 10 MB:   24
Newest Message:     2026-05-29 10:56:48 (ESI-TXN-165-EntryMessage.xml)
Oldest Message:     2026-05-07 08:30:57 (ESI-TXN-119-EntryMessage.xml)


Analysis for .xml messages in: Jun/
-----------------------------------
Total Messages:     33
Largest Message:    1.13 MB (ESI-TXN-186-EntryMessage.xml)
Smallest Message:   24.26 KB (ESI-TXN-187-EntryMessage.xml)
Average Size:       270.87 KB
Messages > 10 MB:   0
Newest Message:     2026-06-27 09:54:41 (ESI-TXN-8-EntryMessage.xml)
Oldest Message:     2026-06-01 10:09:27 (ESI-TXN-166-EntryMessage.xml)

Analysis for .xml messages in: Jul/
-----------------------------------
Total Messages:     1
Largest Message:    24.26 KB (ESI-TXN-192-EntryMessage.xml)
Smallest Message:   24.26 KB (ESI-TXN-192-EntryMessage.xml)
Average Size:       24.26 KB
Messages > 10 MB:   0
Newest Message:     2026-07-22 14:36:31 (ESI-TXN-192-EntryMessage.xml)
Oldest Message:     2026-07-22 14:36:31 (ESI-TXN-192-EntryMessage.xml)

Analysis for .xml messages in: Aug/
-----------------------------------
Total Messages:     12
Largest Message:    1.20 MB (ESI-TXN-201-EntryMessage.xml)
Smallest Message:   64.35 KB (ESI-TXN-196-EntryMessage.xml)
Average Size:       470.18 KB
Messages > 10 MB:   0
Newest Message:     2026-08-11 10:48:58 (ESI-TXN-206-EntryMessage.xml)
Oldest Message:     2026-08-05 11:42:22 (ESI-TXN-193-EntryMessage.xml)


-Within that how many ECN, ECO, etc.  
-% Success and Failure  

79 MCN messages published since February
23 Success
56 Fail
--------
29.1 Success rate, however this includes "re-tries". All the MCN except for Aug were published successfully eventually.


- Avg message size.

Totals:

Analysis for .xml messages in: .
-----------------------------------
Total Messages:     363
Largest Message:    43.85 MB (AMPV_Test1.xml)
Smallest Message:   730 B (ESI-TXN-Blorf-PublicationResultMessage-0000000000000.xml)
Average Size:       3.52 MB
Messages > 10 MB:   56
Newest Message:     2026-08-11 10:49:01 (ESI-TXN-206-PublicationResultMessage-0c196b15-3e34-4cbb-8678-50ee11b8cb88.xml)
Oldest Message:     2026-02-12 21:17:50 (ESI-TXN-8-PublicationResultMessage-5ae079cb-eb93-41d3-9d80-dc47a015b5df.xml)

PTC is asking me: 
1. When was this behavior first observed?

I think we sent our first ESI Release on February 12

2. How many transactions are known to have shown this behavior?

Around 5 I believe (Katie please correct me)

3. What type of release activity has shown this behavior (e.g. BOM components)?

BOM Components 

4. What is the difference in the ESI response between the release activity having “Create” and the one having “Delete”?

One thing we changed was a Soft Attribute (called "OSN") on the usage link I believe. 