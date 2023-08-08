Ez az oldal a getPortfolioData kérés mögötti lekérdezés specifikációját tartalmazza.

```
def : clientId( clientCode : String ) : String = getClient( externalCode == clientCode ).id
def : abl(clientId : string) : Set( AccountBalance ) = getClientAccountBalances( clientId )
def : sbl(clientId : string) : Set( StockBalance ) = -- select * from bml_clavis_stock_details where k.client_id = clientId
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
    clientCode : string = getClient(abl.clientId).externalCode,
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
                instrument : 
                    Tuple(
                        type : {},
                        code : string
                    ),
                valueDate : date,
                bookedBalance : float,
                coverageBalance : float,
                receivableBalance : float,
                fictiveBalance : float,
                liabilityBalance : float
            )
        )=
            abl->collect( abl : AccountBalance | 
                Tuple{
                    account : Tuple{
                        code : string = 
                            getAccount(abl.accountId).accountCode,
                        accountStatus : {} = 
                            getAccount(abl.accountId).status,
                        accountType : {} = 
                            getAccount(abl.accountId).type,
                        accountOpenDate : date =
                            getAccount(abl.accountId).openingDate,
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
                            getAccount(abl.accountId).clientSDIData
                            ->collect(
                                cd : ClientSdiData |
                                Tuple{
                                    code : string = 
                                        cd.directAcc,
                                    type : {} =
                                        cd.transferType,
                                    currency : 
                                        Tuple(
                                            type : {},
                                            code : string
                                        ) =
                                        Tuple{
                                            type : {} = CURRENCY,
                                            code : string = cd.currency
                                        }
                                }
                            )
                    },
                    portfolio : 
                        Tuple{
                            id : integer = abl.portfolioId,
                            name : string = getPortfolioDPM(abl.portfolioId).name,
                            type : {} = getPortfolioGroup(getPortfolioDPM(abl.portfolioId).accountPortfolioGroup).portfolioGroupType,
                            group : 
                                Tuple(
                                    id : integer,
                                    name : string,
                                    status : string,
                                    type : {}
                                ) =
                                Tuple{
                                    id : integer = getPortfolioGroup(getPortfolioDPM(abl.portfolioId).id,
                                    name : string = getPortfolioGroup(getPortfolioDPM(abl.portfolioId).name,
                                    status : string = getPortfolioGroup(getPortfolioDPM(abl.portfolioId).status,
                                    type : {} = getPortfolioGroup(getPortfolioDPM(abl.portfolioId).accountPortfolioGroup).portfolioGroupType
                                }
                        }
                    pool : 
                        Tuple{
                            id : integer = abl.poolId,
                            name : string = getAccountPoolById(abl.poolId).name,
                            type : {} = getAccountPoolById(abl.poolId).accountPoolType
                        }
                    instrument : 
                        Tuple{
                            type : {} = getInstrument(abl.instrument).instrumentType,
                            code : string = getInstrument(abl.instrument).name
                        },
                    valueDate : date = abl.balanceDate,
                    bookedBalance : float = abl.settled,
                    coverageBalance : float = abl.balanceAvailable,
                    receivableBalance : float = abl.balanceAggregated,
                    fictiveBalance : float = abl.fictiveClaim,
                    liabilityBalance : float = abl.liability
                }
            ),
    stocks : Set(
                Tuple(
                    account : 
                        Tuple(
                            code : string = ,
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
         ) = sbl->collect( sbl : StockBalance |
                Tuple{
                    account : 
                        Tuple{
                            code : string = 
                                getAccount(sbl.account_id).accountCode,
                            accountStatus : {} = 
                                getAccount(sbl.account_id).status,
                            accountType : {} = 
                                getAccount(sbl.account_id).type,
                            accountOpenDate : date =
                                getAccount(sbl.account_id).openingDate,
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
                                    getAccount(sbl.account_id).clientSDIData
                                    ->collect(
                                        cd : ClientSdiData |
                                        Tuple{
                                            code : string = 
                                                cd.directAcc,
                                            type : {} =
                                                cd.transferType,
                                            currency : 
                                                Tuple(
                                                    type : {},
                                                    code : string
                                                ) =
                                                Tuple{
                                                    type : {} = CURRENCY,
                                                    code : string = cd.currency
                                                }
                                        }
                                    ),
                        },
                    portfolio : 
                        Tuple{
                            id : integer = sbl.portfolio_id,
                            name : string = getPortfolioDPM(sbl.portfolio_id).name,
                            type : {} = getPortfolioGroup(getPortfolioDPM(sbl.portfolio_id).accountPortfolioGroup).portfolioGroupType,
                            group : 
                                Tuple(
                                    id : integer,
                                    name : string,
                                    status : string,
                                    type : {}
                                ) =
                                Tuple{
                                    id : integer = getPortfolioGroup(getPortfolioDPM(sbl.portfolio_id).id,
                                    name : string = getPortfolioGroup(getPortfolioDPM(sbl.portfolio_id).name,
                                    status : string = getPortfolioGroup(getPortfolioDPM(sbl.portfolio_id).status,
                                    type : {} = getPortfolioGroup(getPortfolioDPM(sbl.portfolio_id).accountPortfolioGroup).portfolioGroupType
                                }
                        },
                    instrumentIsinCode : string = getInstrument(sbl.instrument_id).isin,
                    quantity : float = sbl.all_volume,
                    transactionDate : date = sbl.valueDate,
                    purchaseDate : date = sbl.valueDate,
                    purchaseCurrency : 
                        Tuple(
                            type : {},
                            code : string
                        )=
                        Tuple{
                            type : CURRENCY,
                            code : sbl.transaction_settlement_currency_id
                        }
                    purchaseRate : float = sbl.transaction_ex_rate,
                    settlementDate : date = transaction_settlement_date,
                    mic : string = --bővíteni szükséges a viewt?,
                    status : {} = --bővíteni szükséges a viewt?      
                }
            )
}
```