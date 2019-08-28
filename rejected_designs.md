```elm {v l interactive}
dwellingMap : Spec
dwellingMap =
    let
        data =
            dataFromUrl "./pneumonia_2015_rowcol.csv"

        enc =
            encoding
                << position Y [ pName "Hospital Referral Region (HRR) Description", pMType Ordinal, pAxis [] ]
                << position X [ pName "Average Covered Charges", pMType Quantitative, pAxis [] ]
                << row [ fName "Row", fMType Ordinal, fHeader [ hdTitle "" ] ]
                << column [ fName "Column", fMType Ordinal, fHeader [ hdTitle "" ] ]
                << tooltip [ tName "Hospital Referral Region (HRR) Description", tMType Nominal ]

        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration (coFacet [ facoSpacing 0 ])
    in
    toVegaLite [ data [], cfg [], width 80, height 100, boxplot [], enc [] ]
```

```elm {v l=hidden}
state : String -> Spec
state filename =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])

        data =
            dataFromUrl filename

        enc =
            encoding
                << position X [ pName "Hospital Referral Region (HRR) Description", pMType Nominal, pAxis [ axTitle "" ] ]
                << position Y [ pName "Average Covered Charges", pMType Quantitative ]
    in
    toVegaLite [ width 200, cfg [], data [], boxplot [], enc [] ]
```

|                                |                                |                                |                                |                                |                                |                                |                                |                                |                                |                                |                                |
| ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ | ------------------------------ |
| ^^^elm {v=(state "ak.csv")}^^^ |                                |                                |                                |                                |                                |                                |                                |                                |                                | ^^^elm {v=(state "mi.csv")}^^^ | ^^^elm {v=(state "me.csv")}^^^ |
|                                | ^^^elm {v=(state "wa.csv")}^^^ | ^^^elm {v=(state "id.csv")}^^^ | ^^^elm {v=(state "mt.csv")}^^^ | ^^^elm {v=(state "nd.csv")}^^^ | ^^^elm {v=(state "mn.csv")}^^^ |                                |                                | ^^^elm {v=(state "mi.csv")}^^^ | ^^^elm {v=(state "ny.csv")}^^^ | ^^^elm {v=(state "ri.csv")}^^^ | ^^^elm {v=(state "vt.csv")}^^^ |
|                                | ^^^elm {v=(state "or.csv")}^^^ | ^^^elm {v=(state "ut.csv")}^^^ | ^^^elm {v=(state "wy.csv")}^^^ | ^^^elm {v=(state "sd.csv")}^^^ | ^^^elm {v=(state "ia.csv")}^^^ | ^^^elm {v=(state "wi.csv")}^^^ | ^^^elm {v=(state "in.csv")}^^^ | ^^^elm {v=(state "oh.csv")}^^^ | ^^^elm {v=(state "pa.csv")}^^^ | ^^^elm {v=(state "nj.csv")}^^^ | ^^^elm {v=(state "nh.csv")}^^^ |
|                                | ^^^elm {v=(state "ca.csv")}^^^ | ^^^elm {v=(state "ne.csv")}^^^ | ^^^elm {v=(state "co.csv")}^^^ | ^^^elm {v=(state "ne.csv")}^^^ | ^^^elm {v=(state "mo.csv")}^^^ | ^^^elm {v=(state "il.csv")}^^^ | ^^^elm {v=(state "ky.csv")}^^^ | ^^^elm {v=(state "wv.csv")}^^^ | ^^^elm {v=(state "de.csv")}^^^ | ^^^elm {v=(state "ct.csv")}^^^ |                                |
|                                |                                | ^^^elm {v=(state "az.csv")}^^^ | ^^^elm {v=(state "nm.csv")}^^^ | ^^^elm {v=(state "ks.csv")}^^^ | ^^^elm {v=(state "ar.csv")}^^^ | ^^^elm {v=(state "al.csv")}^^^ | ^^^elm {v=(state "tn.csv")}^^^ | ^^^elm {v=(state "nc.csv")}^^^ | ^^^elm {v=(state "md.csv")}^^^ | ^^^elm {v=(state "dc.csv")}^^^ |                                |
|                                |                                |                                |                                | ^^^elm {v=(state "ok.csv")}^^^ | ^^^elm {v=(state "la.csv")}^^^ | ^^^elm {v=(state "ga.csv")}^^^ | ^^^elm {v=(state "ms.csv")}^^^ | ^^^elm {v=(state "sc.csv")}^^^ | ^^^elm {v=(state "va.csv")}^^^ |                                |                                |
|                                |                                |                                |                                | ^^^elm {v=(state "tx.csv")}^^^ |                                |                                |                                | ^^^elm {v=(state "fl.csv")}^^^ |                                |                                |                                |
| ^^^elm {v=(state "hi.csv")}^^^ |                                |                                |                                |                                |                                |                                |                                |                                |                                |                                |                                |
