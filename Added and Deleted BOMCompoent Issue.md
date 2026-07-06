
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



