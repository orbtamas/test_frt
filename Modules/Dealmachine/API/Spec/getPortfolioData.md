Ez az oldal a getPortfolioData kérés mögötti lekérdezés specifikációját tartalmazza.

```
def : abl : Set( AccountBalance ) = AccountBalance.allInstances()
def : getPortfolioData : Set(  
    Tuple(
        clientCode : string,
        balances : 
            Set(
                Tuple(
                    account : 
                        Tuple(
                            code : string,
                            accountStatus : {},
                            accountType : {},
                            accountOpenDate : date,
                            connectedAccounts : 
                                Set(
                                    Tuple(
                                        code : String,
                                        type : {},
                                        currency : 
                                            Tuple(
                                                type : {},
                                                code : string
                                            )
                                    )
                                ),
                        )
                        ,
                    portfolio : 
                        Tuple(
                            id : integer,
                            name : string,
                            type : {},
                            group : 
                                Tuple(
                                    id : integer,
                                    name : string,
                                    status : string,
                                    type : {}
                                )
                        ),
                    pool : 
                        Tuple(
                            id : integer,
                            name : string,
                            type : {}
                        )
                    instrument : Tuple(
                        type : {},
                        code : string
                    ),
                    valueDate : date,
                    bookedBalance : float,
                    coverageBalance : float,
                    receivableBalance : float,
                    fictiveBalance : float,
                    liabilityBalance : float
                        
                ),
        stocks : Set(
            Tuple(
                account : 
                    Tuple(
                        code : string,
                        accountStatus : {},
                        accountType : {},
                        accountOpenDate : date,
                        connectedAccounts : 
                            Set(
                                Tuple(
                                    code : String,
                                    type : {},
                                    currency : 
                                        Tuple(
                                            type : {},
                                            code : string
                                        )
                                )
                            ),
                    ),
                portfolio : 
                    Tuple(
                        id : integer,
                        name : string,
                        type : {},
                        group : 
                            Tuple(
                                id : integer,
                                name : string,
                                status : string,
                                type : {}
                            )
                    ),
                instrumentIsinCode : string,
                quantity : float,
                transactionDate : date,
                purchaseDate : date,
                purchaseCurrency : 
                    Tuple(
                        type : {},
                        code : string
                    )
                purchaseRate : float,
                settlementDate : date,
                mic : string,
                status : {}            
            )
        )
    ) =
Tuple{
    clientCode : string = abl.account.clientId,
    balances : 
        Set(
            Tuple(
                account : 
                    Tuple(
                        code : string,
                        accountStatus : {},
                        accountType : {},
                        accountOpenDate : date,
                        connectedAccounts : 
                            Set(
                                Tuple(
                                    code : String,
                                    type : {},
                                    currency : 
                                        Tuple(
                                            type : {},
                                            code : string
                                        )
                                )
                            ),
                    ),
                portfolio : 
                    Tuple(
                        id : integer,
                        name : string,
                        type : {},
                        group : 
                            Tuple(
                                id : integer,
                                name : string,
                                status : string,
                                type : {}
                            )
                    ),
                pool : 
                    Tuple(
                        id : integer,
                        name : string,
                        type : {}
                    )
                instrumentIsinCode : string,
                valueDate : date,
                bookedBalance : float,
                coverageBalance : float,
                receivableBalance : float,
                fictiveBalance : float,
                liabilityBalance : float
            )
        )=
            abl.account->collect( acc : account | Tuple{
                code : string = 
                    acc.accCode,
                accountStatus : {} = 
                    acc.status,
                accountType : {} = 
                    clientContracts.allInstances()
                    ->select(
                        cc : ClientContract |
                        cc.id = acc.clientId
                    )
                    ->first().contractType,
                accountOpenDate : date =
                    clientContracts.allInstances()
                    ->select(
                        cc : ClientContract |
                        cc.id = acc.clientId
                    )
                    ->first().validFrom,
                connectedAccounts : 
                    Set(
                        Tuple(
                            code : String,
                            type : {},
                            currency : 
                                Tuple(
                                    type : {},
                                    code : string
                                )
                        )
                    ) = 
                    ConnectedAccountNumber.allInstances()
                    ->select(
                        ca : ConnectedAccountNumber |
                        ca.account = acc.id
                    )
                    ->collect(
                        ca : ConnectedAccountNumber |
                        Tuple{
                            code : string = 
                                ca.sdi.agentAcc,
                            type : {} =
                                ca.sdi.method,
                            currency : 
                                Tuple(
                                    type : {},
                                    code : string
                                ) =
                                Tuple{
                                    type : {} = CURRENCY,
                                    code : string = ca.sdi.currency.code
                                }
                        }
                    ),
                portfolio : 
                    Tuple(
                        id : integer,
                        name : string,
                        type : {},
                        group : 
                            Tuple(
                                id : integer,
                                name : string,
                                status : string,
                                type : {}
                            )
                    )
                    = Tuple{
                        id : integer = 
                            abl.portfolio.id,
                        name : string =
                            abl.portfolio.name,
                        type : {} =
                            abl.portfolio.type,
                        group : 
                            Tuple(
                                id : integer,
                                name : string,
                                status : string,
                                type : {}
                            ) =
                            Tuple{
                                id : integer,
                                name : string,
                                status : string,
                                type : {}
                            }
                    },
                pool : 
                    Tuple(
                        id : integer,
                        name : string,
                        type : {}
                    )
                instrumentIsinCode : string,
                valueDate : date,
                bookedBalance : float,
                coverageBalance : float,
                receivableBalance : float,
                fictiveBalance : float,
                liabilityBalance : float
                    
            )
        }
         ),
    stocks : Set(
        Tuple(
            account : 
                Tuple(
                    code : string,
                    accountStatus : {},
                    accountType : {},
                    accountOpenDate : date,
                    connectedAccounts : 
                        Set(
                            Tuple(
                                code : String,
                                type : {},
                                currency : 
                                    Tuple(
                                        type : {},
                                        code : string
                                    )
                            )
                        ),
                ),
            portfolio : 
                Tuple(
                    id : integer,
                    name : string,
                    type : {},
                    group : 
                        Tuple(
                            id : integer,
                            name : string,
                            status : string,
                            type : {}
                        )
                ),
            instrumentIsinCode : string,
            quantity : float,
            transactionDate : date,
            purchaseDate : date,
            purchaseCurrency : 
                Tuple(
                    type : {},
                    code : string
                )
            purchaseRate : float,
            settlementDate : date,
            mic : string,
            status : {}            
        )
    )
}
```