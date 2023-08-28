# RACH_Msg3_PUSCH_power_Calculator
online calculate: https://dustinchen26.github.io/RACH_Msg3_power

## Ref Spec
```
Ref: 
	ETSI TS 138 213 chap 7.1.1 UE behaviour, 
	ETSI TS 138 331 msg3-DeltaPreamble
```

## Example
```
Enter the following parameters and press "Calculate" to get the RACH Msg3 power
	preambleReceivedTargetPower (from SIB1): -60
	msg3-DeltaPreamble (from SIB1 Actual value = field value * 2): 0
	Po_UE_PUSCH (0: Type-1 random access): 0
	μ (38.211 Table 4.2-1, μ=0 represent 15 kHz, μ=1 represent 30 kHz): 1
	Num_RBs (from [0xB883]): 1
	Pathloss (from [0xB8D2]): 34
	α (if no msgA-Alpha, msg3-Alpha, then α=1): 1
	delta_TF (from [0xB8D2]): 0
	TPC_Adjustment (from [0xB8D2]): -6
	
Calculate

P_PUSCH
	= Po_nominal_PUSCH + Po_UE_PUSCH + 10*log((2^μ)*M_PUSCH_RB) + PL*α + delta_TF + δ_TPC_adjustment
	= (-60 + 2 * 0) + 0 + 10*log((2^1)*1) + 34 * 1 + 0 + -6
	= -28.99 dBm


==============================================================================================================================
P_PUSCH (PUSCH / T_C_RNTI / RACH_Msg3_power)
= Po_nominal_PUSCH + Po_UE_PUSCH + 10*log((2^μ)*M_PUSCH_RB) + PL*α + delta_TF + δ_TPC_adjustment
= (preambleReceivedTargetPower + 2 x msg3-DeltaPreamble) + Po_UE_PUSCH + 10*log((2^μ)*Num_RBs) + Pathloss * α + delta_TF + TPC_Adjustment

07:59:43.496	[0xB821]	BCCH_DL_SCH / SystemInformationBlockType1
            frequencyInfoDL 
            {
              scs-SpecificCarrierList 
              {
                {
                  offsetToCarrier 0,
                  subcarrierSpacing kHz30, //μ=1 (SCS configuration
                  carrierBandwidth 273
                }
              }
              rach-ConfigCommon setup : 
                {
                  rach-ConfigGeneric 
                  {
                    prach-ConfigurationIndex 148,
                    msg1-FDM one,
                    msg1-FrequencyStart 0,
                    zeroCorrelationZoneConfig 6,
                    preambleReceivedTargetPower -60, //preambleReceivedTargetPower=-60
                    preambleTransMax n10,
                    powerRampingStep dB2,
                    ra-ResponseWindow sl80
                  },
                },
              pusch-ConfigCommon setup : 
                {
                  msg3-DeltaPreamble 0, //msg3-DeltaPreamble=0
                  p0-NominalWithGrant -70
                },

// (804,19) PUSCH T_C_RNTI, Num RBs=1
07:59:43.625	[0xB883]	NR5G MAC UL Physical Channel Schedule Report
   ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   |   |                     |       |Carriers                                                                                                                                      
   |   |                     |       |       |              |                                   |      |                                                                            
   |   |                     |       |       |              |                                   |      |PUSCH Data                                                                  
   |   |                     |       |       |              |                                   |      |            |      |       |    |    |                |    |       |       |
   |   |                     |       |       |              |                                   |Dual  |            |      |       |    |    |                |DMRS|       |       |
   |   |System Time          |Num    |Carrier|              |                                   |Pol   |Is Second   |Start |Num    |HARQ|    |                |Add |RB     |       |
   |#  |Slot|Numerology|Frame|Carrier|ID     |RNTI Type     |Phychan Bit Mask                   |Status|Phychan     |Symbol|Symbols|ID  |MCS |MCS Table       |Pos |Start  |Num RBs|
   ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   |  0|  19|     30kHz|  804|      1|      0|      T_C_RNTI|                              PUSCH|     0|           0|     0|     12|   0|   8|           64QAM|   2|      0|      1|

// (804,19) RACH Msg3
07:59:43.631	[0xB88A]	NR5G MAC RACH Attempt
RACH Msg3
   ------------------------------------------------------------------------------------------------------------------
   |   |SFN                |            |Msg3    |     |                                                            |
   |   |      |Sub   |     |Msg3 Grant  |Grant   |HARQ |                                                            |
   |#  |Frame |Frame |Slot |Raw         |Bytes   |Id   |Mac PDU                                                     |
   ------------------------------------------------------------------------------------------------------------------
   |  0|   804|     9|    1|      0x5000|       0|    0|   1D   60   DC   64   26   50   00   00   00   00   00   00|
   
// (804,19) PUSCH Pathloss=34, delta_TF=0, TPC_Adjustment=-6 => PUSCH Transmit Power = -29
07:59:43.644	[0xB8D2]	NR5G LL1 FW MAC TX IU Power
         System Frame Number       = 804
         Slot Number               = 19
      Power Info
         --------------------------------------------------------------------------
         |   |       |       |CarrierIdType                                        
         |   |       |       |PUSCH Data                                          |
         |   |Carrier|Channel|Transmit|        |TPC       |PHR |Delta|Is  |Minimum|
         |#  |Id     |Type   |Power   |Pathloss|Adjustment|MTPL|TF   |Msg3|Power  |
         --------------------------------------------------------------------------
         |  0|      0|  PUSCH|     -29|      34|        -6|  21|    0|   1|    -38|

```
