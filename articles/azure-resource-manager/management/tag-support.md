---
title: Tagga stöd för resurser
description: Visar vilka typer av Azure-resurs typer som stöder taggar. Innehåller information om alla Azure-tjänster.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 6c035f4f9ce6b7c842caeab3b705394417d15ba0
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82609297"
---
# <a name="tag-support-for-azure-resources"></a>Tagga stöd för Azure-resurser
I den här artikeln beskrivs om en resurs typ stöder [taggar](tag-resources.md). Den kolumn som har etiketten **stöder Taggar** anger om resurs typen har en egenskap för taggen. Kolumnen med etiketten **tagg i Cost** visar om den resurs typen skickar taggen till kostnads rapporten. Du kan visa kostnader efter taggar i [Cost Management kostnads analys](../../cost-management-billing/costs/quick-acm-cost-analysis.md#understanding-grouping-and-filtering-options) och Azure- [faktura och användnings data per dag](../../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md).

Hämta samma data som en fil med kommaavgränsade värden genom att ladda ned [tag-support. csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

Hoppa till ett namn område för en resurs leverantör:
> [!div class="op_single_selector"]
> - [Microsoft. AAD](#microsoftaad)
> - [Microsoft. addons](#microsoftaddons)
> - [Microsoft. ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft. Advisor](#microsoftadvisor)
> - [Microsoft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft. AnalysisServices](#microsoftanalysisservices)
> - [Microsoft. API Management](#microsoftapimanagement)
> - [Microsoft. AppConfiguration](#microsoftappconfiguration)
> - [Microsoft. AppPlatform](#microsoftappplatform)
> - [Microsoft. attestering](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft. Automation](#microsoftautomation)
> - [Microsoft. Azconfig](#microsoftazconfig)
> - [Microsoft. Azure. Genève](#microsoftazuregeneva)
> - [Microsoft. AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft. AzureData](#microsoftazuredata)
> - [Microsoft. AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft. fakturering](#microsoftbilling)
> - [Microsoft. Bingkartssökning](#microsoftbingmaps)
> - [Microsoft. blockchain](#microsoftblockchain)
> - [Microsoft. BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft. skiss](#microsoftblueprint)
> - [Microsoft. BotService](#microsoftbotservice)
> - [Microsoft. cache](#microsoftcache)
> - [Microsoft. Capacity](#microsoftcapacity)
> - [Microsoft. CDN](#microsoftcdn)
> - [Microsoft. CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft. ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft. ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft. CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft. Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. förbrukning](#microsoftconsumption)
> - [Microsoft. ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft. ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft. container service](#microsoftcontainerservice)
> - [Microsoft. CostManagement](#microsoftcostmanagement)
> - [Microsoft. CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft. CustomProviders](#microsoftcustomproviders)
> - [Microsoft. data-](#microsoftdatabox)
> - [Microsoft. DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft. Databricks](#microsoftdatabricks)
> - [Microsoft. DataCatalog](#microsoftdatacatalog)
> - [Microsoft. DataFactory](#microsoftdatafactory)
> - [Microsoft. DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft. DataLakeStore](#microsoftdatalakestore)
> - [Microsoft. data migration](#microsoftdatamigration)
> - [Microsoft. DataShare](#microsoftdatashare)
> - [Microsoft. DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft. DBforMySQL](#microsoftdbformysql)
> - [Microsoft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft. DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft. Devices](#microsoftdevices)
> - [Microsoft. DevOps](#microsoftdevops)
> - [Microsoft. DevSpaces](#microsoftdevspaces)
> - [Microsoft. DevTestLab](#microsoftdevtestlab)
> - [Microsoft. DocumentDB](#microsoftdocumentdb)
> - [Microsoft. DomainRegistration](#microsoftdomainregistration)
> - [Microsoft. DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft. EventGrid](#microsofteventgrid)
> - [Microsoft. EventHub](#microsofteventhub)
> - [Microsoft. Falcon](#microsoftfalcon)
> - [Microsoft. features](#microsoftfeatures)
> - [Microsoft. Gallery](#microsoftgallery)
> - [Microsoft. genomik](#microsoftgenomics)
> - [Microsoft. GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft. HanaOnAzure](#microsofthanaonazure)
> - [Microsoft. HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft. HDInsight](#microsofthdinsight)
> - [Microsoft. HealthcareApis](#microsofthealthcareapis)
> - [Microsoft. HybridCompute](#microsofthybridcompute)
> - [Microsoft. HybridData](#microsofthybriddata)
> - [Microsoft. Hydra](#microsofthydra)
> - [Microsoft. ImportExport](#microsoftimportexport)
> - [Microsoft. Insights](#microsoftinsights)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft. IoTCentral](#microsoftiotcentral)
> - [Microsoft. IoTSpaces](#microsoftiotspaces)
> - [Microsoft. nyckel valv](#microsoftkeyvault)
> - [Microsoft. Kubernetes](#microsoftkubernetes)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft. LabServices](#microsoftlabservices)
> - [Microsoft. Logic](#microsoftlogic)
> - [Microsoft. MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft. Maintenance](#microsoftmaintenance)
> - [Microsoft. ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft. ManagedServices](#microsoftmanagedservices)
> - [Microsoft. Management](#microsoftmanagement)
> - [Microsoft. Maps](#microsoftmaps)
> - [Microsoft. Marketplace](#microsoftmarketplace)
> - [Microsoft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft. MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft. Media](#microsoftmedia)
> - [Microsoft. Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft. Migrate](#microsoftmigrate)
> - [Microsoft. MixedReality](#microsoftmixedreality)
> - [Microsoft. NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft. Notebook](#microsoftnotebooks)
> - [Microsoft. NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft. ObjectStore](#microsoftobjectstore)
> - [Microsoft. OffAzure](#microsoftoffazure)
> - [Microsoft. OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft. OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft. peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft. Portal](#microsoftportal)
> - [Microsoft. PowerBI](#microsoftpowerbi)
> - [Microsoft. PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft. ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft. Quantum](#microsoftquantum)
> - [Microsoft. RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft. Relay](#microsoftrelay)
> - [Microsoft. RemoteApp](#microsoftremoteapp)
> - [Microsoft. ResourceGraph](#microsoftresourcegraph)
> - [Microsoft. ResourceHealth](#microsoftresourcehealth)
> - [Microsoft. Resources](#microsoftresources)
> - [Microsoft. SaaS](#microsoftsaas)
> - [Microsoft. search](#microsoftsearch)
> - [Microsoft. Security](#microsoftsecurity)
> - [Microsoft. SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft. SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft. ServiceFabric](#microsoftservicefabric)
> - [Microsoft. ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft. Services](#microsoftservices)
> - [Microsoft. SignalRService](#microsoftsignalrservice)
> - [Microsoft. SiteRecovery](#microsoftsiterecovery)
> - [Microsoft. SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft. Solutions](#microsoftsolutions)
> - [Microsoft. SpoolService](#microsoftspoolservice)
> - [Microsoft. SQL](#microsoftsql)
> - [Microsoft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft. StorageCache](#microsoftstoragecache)
> - [Microsoft. StorageReplication](#microsoftstoragereplication)
> - [Microsoft. StorageSync](#microsoftstoragesync)
> - [Microsoft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft. StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft. StorSimple](#microsoftstorsimple)
> - [Microsoft. StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft. Subscription](#microsoftsubscription)
> - [Microsoft. TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft. VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft. VnfManager](#microsoftvnfmanager)
> - [Microsoft. Web](#microsoftweb)
> - [Microsoft. WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft. WindowsIoT](#microsoftwindowsiot)
> - [Microsoft. WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft. AAD

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | DomainServices | Ja | Ja |
> | DomainServices / oucontainer | Inga | Inga |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | supportProviders | Inga | Inga |

## <a name="microsoftadhybridhealthservice"></a>Microsoft. ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Inga | Inga |
> | addsservices | Inga | Inga |
> | aktörer | Inga | Inga |
> | anonymousapiusers | Inga | Inga |
> | konfiguration | Inga | Inga |
> | loggar | Inga | Inga |
> | rapporter | Inga | Inga |
> | servicehealthmetrics | Inga | Inga |
> | services | Inga | Inga |

## <a name="microsoftadvisor"></a>Microsoft. Advisor

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konfigurationer | Inga | Inga |
> | generateRecommendations | Inga | Inga |
> | metadata | Inga | Inga |
> | rekommendationer | Inga | Inga |
> | utelämningar | Inga | Inga |

## <a name="microsoftalertsmanagement"></a>Microsoft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | actionRules | Ja | Ja |
> | aviseringar | Inga | Inga |
> | alertsList | Inga | Inga |
> | alertsMetaData | Inga | Inga |
> | alertsSummary | Inga | Inga |
> | alertsSummaryList | Inga | Inga |
> | smartDetectorAlertRules | Ja | Ja |
> | smartGroups | Inga | Inga |

## <a name="microsoftanalysisservices"></a>Microsoft. AnalysisServices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | brygghuvudservrar | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft. API Management

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Inga | Inga |
> | tjänst | Ja | Ja |
> | validateServiceName | Inga | Inga |

## <a name="microsoftappconfiguration"></a>Microsoft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | configurationStores | Ja | Ja |
> | configurationStores / eventGridFilters | Inga | Inga |

## <a name="microsoftappplatform"></a>Microsoft. AppPlatform

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Spring | Ja | Ja |

## <a name="microsoftattestation"></a>Microsoft. attestering

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | attestationProviders | Inga | Inga |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | Inga | Inga |
> | dataAliases | Inga | Inga |
> | denyAssignments | Inga | Inga |
> | elevateAccess | Inga | Inga |
> | findOrphanRoleAssignments | Inga | Inga |
> | hålls | Inga | Inga |
> | åtkomst | Inga | Inga |
> | policyAssignments | Inga | Inga |
> | policyDefinitions | Inga | Inga |
> | policySetDefinitions | Inga | Inga |
> | providerOperations | Inga | Inga |
> | roleAssignments | Inga | Inga |
> | roleAssignmentsUsageMetrics | Inga | Inga |
> | roleDefinitions | Inga | Inga |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Ja | Ja |
> | automationAccounts/konfigurationer | Ja | Ja |
> | automationAccounts/jobb | Inga | Inga |
> | automationAccounts / privateEndpointConnectionProxies | Inga | Inga |
> | automationAccounts / privateEndpointConnections | Inga | Inga |
> | automationAccounts / privateLinkResources | Inga | Inga |
> | automationAccounts/Runbooks | Ja | Ja |
> | automationAccounts / softwareUpdateConfigurations | Inga | Inga |
> | automationAccounts/Webhooks | Inga | Inga |

## <a name="microsoftazconfig"></a>Microsoft. Azconfig

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | configurationStores | Ja | Ja |
> | configurationStores / eventGridFilters | Inga | Inga |

## <a name="microsoftazuregeneva"></a>Microsoft. Azure. Genève

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | utrymmen | Inga | Inga |
> | miljöer/konton | Inga | Inga |
> | miljöer/konton/namn områden | Inga | Inga |
> | miljöer/konton/namn rymder/konfigurationer | Inga | Inga |

## <a name="microsoftazureactivedirectory"></a>Microsoft. AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Ja | Inga |
> | b2ctenants | Inga | Inga |

## <a name="microsoftazuredata"></a>Microsoft. AzureData

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | hybridDataManagers | Ja | Ja |
> | postgresInstances | Ja | Ja |
> | sqlInstances | Ja | Ja |
> | sqlServerRegistrations | Ja | Ja |
> | sqlServerRegistrations/sqlServer | Inga | Inga |

## <a name="microsoftazurestack"></a>Microsoft. AzureStack

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | cloudManifestFiles | Inga | Inga |
> | registreringar | Ja | Ja |
> | registreringar/customerSubscriptions | Inga | Inga |
> | registreringar/produkter | Inga | Inga |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Ja | Ja |

## <a name="microsoftbilling"></a>Microsoft. fakturering

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | billingAccounts | Inga | Inga |
> | billingAccounts/avtal | Inga | Inga |
> | billingAccounts / billingPermissions | Inga | Inga |
> | billingAccounts / billingProfiles | Inga | Inga |
> | billingAccounts / billingProfiles / billingPermissions | Inga | Inga |
> | billingAccounts / billingProfiles / billingRoleAssignments | Inga | Inga |
> | billingAccounts / billingProfiles / billingRoleDefinitions | Inga | Inga |
> | billingAccounts / billingProfiles / billingSubscriptions | Inga | Inga |
> | billingAccounts / billingProfiles / createBillingRoleAssignment | Inga | Inga |
> | billingAccounts/billingProfiles/kunder | Inga | Inga |
> | billingAccounts/billingProfiles/instruktioner | Inga | Inga |
> | billingAccounts/billingProfiles/fakturor | Inga | Inga |
> | billingAccounts/billingProfiles/fakturor/pris dokument | Inga | Inga |
> | billingAccounts/billingProfiles/fakturor/transaktioner | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections / billingPermissions | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleAssignments | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections / billingRoleDefinitions | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections / billingSubscriptions | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections / createBillingRoleAssignment | Inga | Inga |
> | billingAccounts / billingProfiles / invoiceSections / initiateTransfer | Inga | Inga |
> | billingAccounts/billingProfiles/invoiceSections/Products | Inga | Inga |
> | billingAccounts/billingProfiles/invoiceSections/Products/transfer | Inga | Inga |
> | billingAccounts/billingProfiles/invoiceSections/Products/updateAutoRenew | Inga | Inga |
> | billingAccounts/billingProfiles/invoiceSections/transaktioner | Inga | Inga |
> | billingAccounts/billingProfiles/invoiceSections/transfers | Inga | Inga |
> | billingAccounts / BillingProfiles / patchOperations | Inga | Inga |
> | billingAccounts / billingProfiles / paymentMethods | Inga | Inga |
> | billingAccounts/billingProfiles/policys | Inga | Inga |
> | billingAccounts/billingProfiles/pris dokument | Inga | Inga |
> | billingAccounts / billingProfiles / pricesheetDownloadOperations | Inga | Inga |
> | billingAccounts/billingProfiles/Products | Inga | Inga |
> | billingAccounts/billingProfiles/transaktioner | Inga | Inga |
> | billingAccounts / billingRoleAssignments | Inga | Inga |
> | billingAccounts / billingRoleDefinitions | Inga | Inga |
> | billingAccounts / billingSubscriptions | Inga | Inga |
> | billingAccounts/billingSubscriptions/fakturor | Inga | Inga |
> | billingAccounts / createBillingRoleAssignment | Inga | Inga |
> | billingAccounts / createInvoiceSectionOperations | Inga | Inga |
> | billingAccounts/kunder | Inga | Inga |
> | billingAccounts/kunder/billingPermissions | Inga | Inga |
> | billingAccounts/kunder/billingSubscriptions | Inga | Inga |
> | billingAccounts/kunder/initiateTransfer | Inga | Inga |
> | billingAccounts/kunder/principer | Inga | Inga |
> | billingAccounts/kunder/produkter | Inga | Inga |
> | billingAccounts/kunder/transaktioner | Inga | Inga |
> | billingAccounts/kunder/överföringar | Inga | Inga |
> | billingAccounts/avdelningar | Inga | Inga |
> | billingAccounts / enrollmentAccounts | Inga | Inga |
> | billingAccounts/fakturor | Inga | Inga |
> | billingAccounts / invoiceSections | Inga | Inga |
> | billingAccounts / invoiceSections / billingSubscriptionMoveOperations | Inga | Inga |
> | billingAccounts / invoiceSections / billingSubscriptions | Inga | Inga |
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Inga | Inga |
> | billingAccounts/invoiceSections/höjning | Inga | Inga |
> | billingAccounts / invoiceSections / initiateTransfer | Inga | Inga |
> | billingAccounts / invoiceSections / patchOperations | Inga | Inga |
> | billingAccounts / invoiceSections / productMoveOperations | Inga | Inga |
> | billingAccounts/invoiceSections/Products | Inga | Inga |
> | billingAccounts/invoiceSections/Products/transfer | Inga | Inga |
> | billingAccounts/invoiceSections/Products/updateAutoRenew | Inga | Inga |
> | billingAccounts/invoiceSections/transaktioner | Inga | Inga |
> | billingAccounts/invoiceSections/överföringar | Inga | Inga |
> | billingAccounts / lineOfCredit | Inga | Inga |
> | billingAccounts / patchOperations | Inga | Inga |
> | billingAccounts / paymentMethods | Inga | Inga |
> | billingAccounts/produkter | Inga | Inga |
> | billingAccounts/transaktioner | Inga | Inga |
> | billingPeriods | Inga | Inga |
> | billingPermissions | Inga | Inga |
> | billingProperty | Inga | Inga |
> | billingRoleAssignments | Inga | Inga |
> | billingRoleDefinitions | Inga | Inga |
> | createBillingRoleAssignment | Inga | Inga |
> | enheten | Inga | Inga |
> | enrollmentAccounts | Inga | Inga |
> | fakturor | Inga | Inga |
> | överlåtelse | Inga | Inga |
> | överföringar/acceptTransfer | Inga | Inga |
> | överföringar/declineTransfer | Inga | Inga |
> | överföringar/operationStatus | Inga | Inga |
> | överföringar/validateTransfer | Inga | Inga |
> | validateAddress | Inga | Inga |

## <a name="microsoftbingmaps"></a>Microsoft. Bingkartssökning

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | mapApis | Ja | Ja |
> | updateCommunicationPreference | Inga | Inga |

## <a name="microsoftblockchain"></a>Microsoft. blockchain

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Ja | Ja |
> | cordaMembers | Ja | Ja |
> | Övervakare | Ja | Ja |

## <a name="microsoftblockchaintokens"></a>Microsoft. BlockchainTokens

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | TokenServices | Ja | Ja |
> | TokenServices / BlockchainNetworks | Inga | Inga |
> | TokenServices/grupper | Inga | Inga |
> | TokenServices/grupper/konton | Inga | Inga |
> | TokenServices / TokenTemplates | Inga | Inga |

## <a name="microsoftblueprint"></a>Microsoft. skiss

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | blueprintAssignments | Inga | Inga |
> | blueprintAssignments / assignmentOperations | Inga | Inga |
> | blueprintAssignments/åtgärder | Inga | Inga |
> | modeller | Inga | Inga |
> | skisser/artefakter | Inga | Inga |
> | skisser/versioner | Inga | Inga |
> | skisser/versioner/artefakter | Inga | Inga |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | botServices | Ja | Ja |
> | botServices/kanaler | Inga | Inga |
> | botServices/anslutningar | Inga | Inga |
> | språk | Inga | Inga |
> | mallar | Inga | Inga |

## <a name="microsoftcache"></a>Microsoft. cache

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Redis | Ja | Ja |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | appliedReservations | Inga | Inga |
> | autoQuotaIncrease | Inga | Inga |
> | calculateExchange | Inga | Inga |
> | calculatePrice | Inga | Inga |
> | calculatePurchasePrice | Inga | Inga |
> | kataloger | Inga | Inga |
> | commercialReservationOrders | Inga | Inga |
> | utväxla | Inga | Inga |
> | placePurchaseOrder | Inga | Inga |
> | reservationOrders | Inga | Inga |
> | reservationOrders / calculateRefund | Inga | Inga |
> | reservationOrders/slå samman | Inga | Inga |
> | reservationOrders/reservationer | Inga | Inga |
> | reservationOrders/reservationer/revisioner | Inga | Inga |
> | reservationOrders/retur | Inga | Inga |
> | reservationOrders/Split | Inga | Inga |
> | reservationOrders/växling | Inga | Inga |
> | reservera | Inga | Inga |
> | resourceProviders | Inga | Inga |
> | resources | Inga | Inga |
> | validateReservationOrder | Inga | Inga |

## <a name="microsoftcdn"></a>Microsoft. CDN

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Inga | Inga |
> | CdnWebApplicationFirewallPolicies | Ja | Ja |
> | edgenodes | Inga | Inga |
> | filer | Ja | Ja |
> | profiler/slut punkter | Ja | Ja |
> | profiler/slut punkter/customdomains | Inga | Inga |
> | profiler/slut punkter/origingroups | Inga | Inga |
> | profiler/slut punkter/ursprung | Inga | Inga |
> | validateProbe | Inga | Inga |

## <a name="microsoftcertificateregistration"></a>Microsoft. CertificateRegistration

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | certificateOrders | Ja | Ja |
> | certificateOrders/certifikat | Inga | Inga |
> | validateCertificateRegistrationInformation | Inga | Inga |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | funktioner eller behörigheter | Inga | Inga |
> | Domän namn | Inga | Inga |
> | Domän namn/funktioner | Inga | Inga |
> | Domän namn/internalLoadBalancers | Inga | Inga |
> | Domän namn/serviceCertificates | Inga | Inga |
> | Domän namn/platser | Inga | Inga |
> | Domän namn/platser/roller | Inga | Inga |
> | Domän namn/platser/roller/metricDefinitions | Inga | Inga |
> | Domän namn/platser/roller/mått | Inga | Inga |
> | moveSubscriptionResources | Inga | Inga |
> | operatingSystemFamilies | Inga | Inga |
> | operatingSystems | Inga | Inga |
> | quotas | Inga | Inga |
> | resourceTypes | Inga | Inga |
> | validateSubscriptionMoveAvailability | Inga | Inga |
> | virtualMachines | Inga | Inga |
> | virtualMachines / diagnosticSettings | Inga | Inga |
> | virtualMachines / metricDefinitions | Inga | Inga |
> | virtualMachines/mått | Inga | Inga |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | Inga | Inga |

## <a name="microsoftclassicnetwork"></a>Microsoft. ClassicNetwork

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | funktioner eller behörigheter | Inga | Inga |
> | expressRouteCrossConnections | Inga | Inga |
> | expressRouteCrossConnections/peering | Inga | Inga |
> | gatewaySupportedDevices | Inga | Inga |
> | networkSecurityGroups | Inga | Inga |
> | quotas | Inga | Inga |
> | reservedIps | Inga | Inga |
> | virtualNetworks | Inga | Inga |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Inga | Inga |
> | virtualNetworks/virtualNetworkPeerings | Inga | Inga |

## <a name="microsoftclassicstorage"></a>Microsoft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | funktioner eller behörigheter | Inga | Inga |
> | disk | Inga | Inga |
> | images | Inga | Inga |
> | osImages | Inga | Inga |
> | osPlatformImages | Inga | Inga |
> | publicImages | Inga | Inga |
> | quotas | Inga | Inga |
> | storageAccounts | Inga | Inga |
> | storageAccounts/blobServices | Inga | Inga |
> | storageAccounts/fileServices | Inga | Inga |
> | storageAccounts/metricDefinitions | Inga | Inga |
> | storageAccounts/mått | Inga | Inga |
> | storageAccounts/queueServices | Inga | Inga |
> | storageAccounts/tjänster | Inga | Inga |
> | storageAccounts/tjänster/diagnosticSettings | Inga | Inga |
> | storageAccounts/tjänster/metricDefinitions | Inga | Inga |
> | storageAccounts/tjänster/mått | Inga | Inga |
> | storageAccounts/tableServices | Inga | Inga |
> | storageAccounts/vmImages | Inga | Inga |
> | vmImages | Inga | Inga |

## <a name="microsoftcognitiveservices"></a>Microsoft. CognitiveServices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | RateCard | Inga | Inga |
> | UsageAggregates | Inga | Inga |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Ja | Ja |
> | diskEncryptionSets | Ja | Ja |
> | disk | Ja | Ja |
> | gallerier | Ja | Ja |
> | gallerier/program | Inga | Inga |
> | gallerier/program/versioner | Inga | Inga |
> | gallerier/bilder | Inga | Inga |
> | gallerier/avbildningar/versioner | Inga | Inga |
> | hostGroups | Ja | Ja |
> | hostGroups/värdar | Ja | Ja |
> | images | Ja | Ja |
> | proximityPlacementGroups | Ja | Ja |
> | restorePointCollections | Ja | Ja |
> | restorePointCollections / restorePoints | Inga | Inga |
> | sharedVMExtensions | Ja | Ja |
> | sharedVMExtensions/versioner | Inga | Inga |
> | sharedVMImages | Ja | Ja |
> | sharedVMImages/versioner | Inga | Inga |
> | snapshots | Ja | Ja |
> | sshPublicKeys | Ja | Ja |
> | virtualMachines | Ja | Ja |
> | virtualMachines/tillägg | Ja | Ja |
> | virtualMachines / metricDefinitions | Inga | Inga |
> | virtualMachineScaleSets | Ja | Ja |
> | virtualMachineScaleSets/tillägg | Inga | Inga |
> | virtualMachineScaleSets/networkInterfaces | Inga | Inga |
> | virtualMachineScaleSets/publicIPAddresses | Inga | Inga |
> | virtualMachineScaleSets/virtualMachines | Inga | Inga |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Inga | Inga |

## <a name="microsoftconsumption"></a>Microsoft. förbrukning

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | AggregatedCost | Inga | Inga |
> | Saldon | Inga | Inga |
> | Budgetar | Inga | Inga |
> | Avgifter | Inga | Inga |
> | CostTags | Inga | Inga |
> | krediter | Inga | Inga |
> | händelser | Inga | Inga |
> | Prognoser | Inga | Inga |
> | samtliga | Inga | Inga |
> | Marknads platser | Inga | Inga |
> | Prisdokument | Inga | Inga |
> | läkemedle | Inga | Inga |
> | ReservationDetails | Inga | Inga |
> | ReservationRecommendations | Inga | Inga |
> | ReservationSummaries | Inga | Inga |
> | ReservationTransactions | Inga | Inga |
> | Taggar | Inga | Inga |
> | klienter | Inga | Inga |
> | Termer | Inga | Inga |
> | UsageDetails | Inga | Inga |

## <a name="microsoftcontainerinstance"></a>Microsoft. ContainerInstance

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | containerGroups | Ja | Ja |
> | serviceAssociationLinks | Inga | Inga |

## <a name="microsoftcontainerregistry"></a>Microsoft. ContainerRegistry

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | register | Ja | Ja |
> | register/agentPools | Ja | Ja |
> | register/versioner | Inga | Inga |
> | register/versioner/Avbryt | Inga | Inga |
> | register/build/getLogLink | Inga | Inga |
> | register/buildTasks | Ja | Ja |
> | register/buildTasks/steg | Inga | Inga |
> | register/eventGridFilters | Inga | Inga |
> | register/generateCredentials | Inga | Inga |
> | register/getBuildSourceUploadUrl | Inga | Inga |
> | register/GetCredentials | Inga | Inga |
> | register/importImage | Inga | Inga |
> | register/privateEndpointConnectionProxies | Inga | Inga |
> | register/privateEndpointConnectionProxies/validate | Inga | Inga |
> | register/privateEndpointConnections | Inga | Inga |
> | register/privateLinkResources | Inga | Inga |
> | register/queueBuild | Inga | Inga |
> | register/regenerateCredential | Inga | Inga |
> | register/regenerateCredentials | Inga | Inga |
> | register/replikeringar | Ja | Ja |
> | register/körningar | Inga | Inga |
> | register/körningar/Avbryt | Inga | Inga |
> | register/scheduleRun | Inga | Inga |
> | register/scopeMaps | Inga | Inga |
> | register/taskRuns | Ja | Ja |
> | register/uppgifter | Ja | Ja |
> | register/token | Inga | Inga |
> | register/updatePolicies | Inga | Inga |
> | register/Webhooks | Ja | Ja |
> | register/Webhooks/getCallbackConfig | Inga | Inga |
> | register/Webhooks/ping | Inga | Inga |

## <a name="microsoftcontainerservice"></a>Microsoft. container service

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | containerServices | Ja | Ja |
> | managedClusters | Ja | Ja |
> | openShiftManagedClusters | Ja | Ja |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Aviseringar | Inga | Inga |
> | BillingAccounts | Inga | Inga |
> | Budgetar | Inga | Inga |
> | CloudConnectors | Inga | Inga |
> | Anslutningar | Ja | Ja |
> | Avdelningar | Inga | Inga |
> | Dimensioner | Inga | Inga |
> | EnrollmentAccounts | Inga | Inga |
> | Exporteras | Inga | Inga |
> | ExternalBillingAccounts | Inga | Inga |
> | ExternalBillingAccounts/aviseringar | Inga | Inga |
> | ExternalBillingAccounts/dimensioner | Inga | Inga |
> | ExternalBillingAccounts/prognos | Inga | Inga |
> | ExternalBillingAccounts/fråga | Inga | Inga |
> | ExternalSubscriptions | Inga | Inga |
> | ExternalSubscriptions/aviseringar | Inga | Inga |
> | ExternalSubscriptions/dimensioner | Inga | Inga |
> | ExternalSubscriptions/prognos | Inga | Inga |
> | ExternalSubscriptions/fråga | Inga | Inga |
> | Prognos | Inga | Inga |
> | Söka i data | Inga | Inga |
> | registrera | Inga | Inga |
> | Reportconfigs | Inga | Inga |
> | Rapporter | Inga | Inga |
> | Inställningar | Inga | Inga |
> | showbackRules | Inga | Inga |
> | Vyer | Inga | Inga |

## <a name="microsoftcustomerlockbox"></a>Microsoft. CustomerLockbox

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | autentiseringsbegäran | Inga | Inga |

## <a name="microsoftcustomproviders"></a>Microsoft. CustomProviders

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | typer | Inga | Inga |
> | resourceProviders | Ja | Ja |

## <a name="microsoftdatabox"></a>Microsoft. data-

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | utskrifts | Ja | Ja |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Ja | Ja |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | arbetsytor | Ja | Inga |
> | arbets ytor/dbWorkspaces | Inga | Inga |
> | arbets ytor/storageEncryption | Inga | Inga |
> | arbets ytor/virtualNetworkPeerings | Inga | Inga |

## <a name="microsoftdatacatalog"></a>Microsoft. DataCatalog

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | kataloger | Ja | Ja |
> | datacatalogs | Ja | Ja |
> | datacatalogs/data källor | Inga | Inga |
> | datacatalogs/data källor/-genomsökningar | Inga | Inga |
> | datacatalogs/data källor/genomsökningar/data uppsättningar | Inga | Inga |
> | datacatalogs/data källor/genomsökningar/utlösare | Inga | Inga |

## <a name="microsoftdatafactory"></a>Microsoft. DataFactory

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | dataFactories | Ja | Inga |
> | dataFactories / diagnosticSettings | Inga | Inga |
> | dataFactories / metricDefinitions | Inga | Inga |
> | dataFactorySchema | Inga | Inga |
> | fabriker | Ja | Inga |
> | fabriker/integrationRuntimes | Inga | Inga |

## <a name="microsoftdatalakeanalytics"></a>Microsoft. DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |
> | konton/dataLakeStoreAccounts | Inga | Inga |
> | konton/storageAccounts | Inga | Inga |
> | konton/storageAccounts/behållare | Inga | Inga |
> | konton/transferAnalyticsUnits | Inga | Inga |

## <a name="microsoftdatalakestore"></a>Microsoft. DataLakeStore

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |
> | konton/eventGridFilters | Inga | Inga |
> | konton/firewallRules | Inga | Inga |

## <a name="microsoftdatamigration"></a>Microsoft. data migration

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | services | Inga | Inga |
> | tjänster/projekt | Inga | Inga |

## <a name="microsoftdatashare"></a>Microsoft. DataShare

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |
> | konton/resurser | Inga | Inga |
> | konton/resurser/data uppsättningar | Inga | Inga |
> | konton/resurser/inbjudningar | Inga | Inga |
> | konton/resurser/providersharesubscriptions | Inga | Inga |
> | konton/resurser/synchronizationSettings | Inga | Inga |
> | konton/sharesubscriptions | Inga | Inga |
> | konton/sharesubscriptions/consumerSourceDataSets | Inga | Inga |
> | konton/sharesubscriptions/datasetmappings | Inga | Inga |
> | konton/sharesubscriptions/utlösare | Inga | Inga |

## <a name="microsoftdbformariadb"></a>Microsoft. DBforMariaDB

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | brygghuvudservrar | Ja | Ja |
> | servrar/rådgivare | Inga | Inga |
> | servrar/nycklar | Inga | Inga |
> | servrar/privateEndpointConnectionProxies | Inga | Inga |
> | servrar/privateEndpointConnections | Inga | Inga |
> | servrar/privateLinkResources | Inga | Inga |
> | servrar/queryTexts | Inga | Inga |
> | servrar/recoverableServers | Inga | Inga |
> | servrar/topQueryStatistics | Inga | Inga |
> | servrar/virtualNetworkRules | Inga | Inga |
> | servrar/waitStatistics | Inga | Inga |

## <a name="microsoftdbformysql"></a>Microsoft. DBforMySQL

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | brygghuvudservrar | Ja | Ja |
> | servrar/rådgivare | Inga | Inga |
> | servrar/nycklar | Inga | Inga |
> | servrar/privateEndpointConnectionProxies | Inga | Inga |
> | servrar/privateEndpointConnections | Inga | Inga |
> | servrar/privateLinkResources | Inga | Inga |
> | servrar/queryTexts | Inga | Inga |
> | servrar/recoverableServers | Inga | Inga |
> | servrar/topQueryStatistics | Inga | Inga |
> | servrar/virtualNetworkRules | Inga | Inga |
> | servrar/waitStatistics | Inga | Inga |

## <a name="microsoftdbforpostgresql"></a>Microsoft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | serverGroups | Ja | Ja |
> | brygghuvudservrar | Ja | Ja |
> | servrar/rådgivare | Inga | Inga |
> | servrar/nycklar | Inga | Inga |
> | servrar/privateEndpointConnectionProxies | Inga | Inga |
> | servrar/privateEndpointConnections | Inga | Inga |
> | servrar/privateLinkResources | Inga | Inga |
> | servrar/queryTexts | Inga | Inga |
> | servrar/recoverableServers | Inga | Inga |
> | servrar/topQueryStatistics | Inga | Inga |
> | servrar/virtualNetworkRules | Inga | Inga |
> | servrar/waitStatistics | Inga | Inga |
> | serversv2 | Ja | Ja |
> | singleServers | Ja | Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | artifactSources | Ja | Ja |
> | distributioner | Ja | Ja |
> | serviceTopologies | Ja | Ja |
> | serviceTopologies/tjänster | Ja | Ja |
> | serviceTopologies/tjänster/serviceUnits | Ja | Ja |
> | steg | Ja | Ja |

## <a name="microsoftdesktopvirtualization"></a>Microsoft. DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Ja | Ja |
> | applicationgroups/program | Inga | Inga |
> | applicationgroups/Station ära datorer | Inga | Inga |
> | applicationgroups / startmenuitems | Inga | Inga |
> | hostpools | Ja | Ja |
> | hostpools / sessionhosts | Inga | Inga |
> | hostpools / sessionhosts / usersessions | Inga | Inga |
> | hostpools / usersessions | Inga | Inga |
> | arbetsytor | Ja | Ja |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | ElasticPools | Ja | Ja |
> | ElasticPools / IotHubTenants | Ja | Ja |
> | ElasticPools/IotHubTenants/securitySettings | Inga | Inga |
> | IotHubs | Ja | Ja |
> | IotHubs / eventGridFilters | Inga | Inga |
> | IotHubs/securitySettings | Inga | Inga |
> | ProvisioningServices | Ja | Ja |
> | användningar | Inga | Inga |

## <a name="microsoftdevops"></a>Microsoft. DevOps

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | pipelines | Ja | Ja |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | domänkontrollanter | Ja | Ja |

## <a name="microsoftdevtestlab"></a>Microsoft. DevTestLab

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | labcenters | Ja | Ja |
> | Laboration | Ja | Ja |
> | labb/miljöer | Ja | Ja |
> | labb/serviceRunners | Ja | Ja |
> | labb/virtualMachines | Ja | Ja |
> | scheman | Ja | Ja |

## <a name="microsoftdocumentdb"></a>Microsoft. DocumentDB

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | Inga | Inga |
> | databaseAccounts | Ja | Ja |

## <a name="microsoftdomainregistration"></a>Microsoft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | domäner | Ja | Ja |
> | domäner/domainOwnershipIdentifiers | Inga | Inga |
> | generateSsoRequest | Inga | Inga |
> | topLevelDomains | Inga | Inga |
> | validateDomainRegistrationInformation | Inga | Inga |

## <a name="microsoftdynamicslcs"></a>Microsoft. DynamicsLcs

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Inga | Inga |
> | lcsprojects / clouddeployments | Inga | Inga |
> | lcsprojects/kopplingar | Inga | Inga |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | services | Ja | Ja |

## <a name="microsofteventgrid"></a>Microsoft. EventGrid

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | domäner | Ja | Ja |
> | domäner/ämnen | Inga | Inga |
> | eventSubscriptions | Inga | Inga |
> | extensionTopics | Inga | Inga |
> | partnerNamespaces | Ja | Ja |
> | partnerNamespaces/eventChannels | Inga | Inga |
> | partnerRegistrations | Ja | Ja |
> | partnerTopics | Ja | Ja |
> | partnerTopics / eventSubscriptions | Inga | Inga |
> | systemTopics | Ja | Ja |
> | systemTopics / eventSubscriptions | Inga | Inga |
> | avsnitt | Ja | Ja |
> | topicTypes | Inga | Inga |

## <a name="microsofteventhub"></a>Microsoft. EventHub

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | kluster | Ja | Ja |
> | namn områden | Ja | Ja |
> | namnrymder/authorizationrules | Inga | Inga |
> | namnrymder/disasterrecoveryconfigs | Inga | Inga |
> | namnrymder/eventhubs | Inga | Inga |
> | namnrymder/eventhubs/authorizationrules | Inga | Inga |
> | namnrymder/eventhubs/consumergroups | Inga | Inga |
> | namnrymder/networkrulesets | Inga | Inga |

## <a name="microsoftfalcon"></a>Microsoft. Falcon

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | namn områden | Ja | Ja |

## <a name="microsoftfeatures"></a>Microsoft. features

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | featureProviders | Inga | Inga |
> | funktioner | Inga | Inga |
> | finansiär | Inga | Inga |
> | subscriptionFeatureRegistrations | Inga | Inga |

## <a name="microsoftgallery"></a>Microsoft. Gallery

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | certifiering | Inga | Inga |
> | galleryitems | Inga | Inga |
> | generateartifactaccessuri | Inga | Inga |
> | områden | Inga | Inga |
> | område/områden | Inga | Inga |
> | område/områden/områden | Inga | Inga |
> | område/områden/områden/galleryitems | Inga | Inga |
> | områdets/områden/galleryitems | Inga | Inga |
> | områden/galleryitems | Inga | Inga |
> | registrera | Inga | Inga |
> | resources | Inga | Inga |
> | retrieveresourcesbyid | Inga | Inga |

## <a name="microsoftgenomics"></a>Microsoft. genomik

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | autoManagedAccounts | Ja | Ja |
> | autoManagedVmConfigurationProfiles | Ja | Ja |
> | configurationProfileAssignments | Inga | Inga |
> | guestConfigurationAssignments | Inga | Inga |
> | IntelliPoint | Inga | Inga |
> | softwareUpdateProfile | Inga | Inga |
> | softwareUpdates | Inga | Inga |

## <a name="microsofthanaonazure"></a>Microsoft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | hanaInstances | Ja | Ja |
> | sapMonitors | Ja | Ja |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft. HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | dedicatedHSMs | Ja | Ja |

## <a name="microsofthdinsight"></a>Microsoft. HDInsight

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | kluster | Ja | Ja |
> | kluster/program | Inga | Inga |

## <a name="microsofthealthcareapis"></a>Microsoft. HealthcareApis

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | services | Ja | Ja |

## <a name="microsofthybridcompute"></a>Microsoft. HybridCompute

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | faxar | Ja | Ja |
> | datorer/tillägg | Ja | Ja |

## <a name="microsofthybriddata"></a>Microsoft. HybridData

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | dataManagers | Ja | Ja |

## <a name="microsofthydra"></a>Microsoft. Hydra

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | delarna | Ja | Ja |
> | networkScopes | Ja | Ja |

## <a name="microsoftimportexport"></a>Microsoft. ImportExport

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | utskrifts | Ja | Ja |

## <a name="microsoftinsights"></a>Microsoft. Insights

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | actionGroups | Ja | Ja |
> | activityLogAlerts | Ja | Ja |
> | alertrules | Ja | Ja |
> | autoscalesettings | Ja | Ja |
> | delarna | Ja | Ja |
> | komponenter/linkedStorageAccounts | Inga | Inga |
> | komponenter/ProactiveDetectionConfigs | Inga | Inga |
> | diagnosticSettings | Inga | Inga |
> | guestDiagnosticSettings | Ja | Ja |
> | guestDiagnosticSettingsAssociation | Ja | Ja |
> | logprofiles | Ja | Ja |
> | metricAlerts | Ja | Ja |
> | privateLinkScopes | Ja | Ja |
> | privateLinkScopes / privateEndpointConnections | Inga | Inga |
> | privateLinkScopes / scopedResources | Inga | Inga |
> | queryPacks | Ja | Ja |
> | queryPacks/frågor | Inga | Inga |
> | scheduledQueryRules | Ja | Ja |
> | webbtester | Ja | Ja |
> | arbetsböcker | Ja | Ja |
> | workbooktemplates | Ja | Ja |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Inga | Inga |
> | diagnosticSettingsCategories | Inga | Inga |

## <a name="microsoftiotcentral"></a>Microsoft. IoTCentral

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | appTemplates | Inga | Inga |
> | IoTApps | Ja | Ja |

## <a name="microsoftiotspaces"></a>Microsoft. IoTSpaces

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Graph | Ja | Ja |

## <a name="microsoftkeyvault"></a>Microsoft. nyckel valv

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | deletedVaults | Inga | Inga |
> | hsmPools | Ja | Ja |
> | valv | Ja | Ja |
> | valv/accessPolicies | Inga | Inga |
> | valv/eventGridFilters | Inga | Inga |
> | valv/hemligheter | Inga | Inga |

## <a name="microsoftkubernetes"></a>Microsoft. Kubernetes

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | connectedClusters | Ja | Ja |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | kluster | Ja | Ja |
> | kluster/attacheddatabaseconfigurations | Inga | Inga |
> | kluster/databaser | Inga | Inga |
> | kluster/databaser/dataconnections | Inga | Inga |
> | kluster/databaser/eventhubconnections | Inga | Inga |
> | kluster/databaser/principalassignments | Inga | Inga |
> | kluster/dataconnections | Inga | Inga |
> | kluster/principalassignments | Inga | Inga |
> | kluster/sharedidentities | Inga | Inga |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | labaccounts | Ja | Ja |
> | användare | Inga | Inga |

## <a name="microsoftlogic"></a>Microsoft. Logic

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Ja | Ja |
> | integrationAccounts | Ja | Ja |
> | integrationServiceEnvironments | Ja | Ja |
> | integrationServiceEnvironments/managedApis | Ja | Ja |
> | isolatedEnvironments | Ja | Ja |
> | arbetsflöden | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft. MachineLearning

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | commitmentPlans | Ja | Ja |
> | WebServices | Ja | Ja |
> | Arbetsytor | Ja | Ja |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | arbetsytor | Ja | Ja |
> | arbets ytor/beräkningar | Inga | Inga |
> | arbets ytor/eventGridFilters | Inga | Inga |

## <a name="microsoftmaintenance"></a>Microsoft. Maintenance

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | applyUpdates | Inga | Inga |
> | configurationAssignments | Inga | Inga |
> | maintenanceConfigurations | Ja | Ja |
> | uppdateringar | Inga | Inga |

## <a name="microsoftmanagedidentity"></a>Microsoft. ManagedIdentity

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Identiteter | Inga | Inga |
> | userAssignedIdentities | Ja | Ja |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | marketplaceRegistrationDefinitions | Inga | Inga |
> | registrationAssignments | Inga | Inga |
> | registrationDefinitions | Inga | Inga |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | getEntities | Inga | Inga |
> | managementGroups | Inga | Inga |
> | managementGroups/inställningar | Inga | Inga |
> | resources | Inga | Inga |
> | startTenantBackfill | Inga | Inga |
> | tenantBackfillStatus | Inga | Inga |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |
> | konton/eventGridFilters | Inga | Inga |
> | konton/privateAtlases | Ja | Ja |

## <a name="microsoftmarketplace"></a>Microsoft. Marketplace

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | budgivning | Inga | Inga |
> | offerTypes | Inga | Inga |
> | offerTypes/utgivare | Inga | Inga |
> | offerTypes/utgivare/erbjudanden | Inga | Inga |
> | offerTypes/utgivare/erbjudanden/planer | Inga | Inga |
> | offerTypes/utgivare/erbjudanden/planer/avtal | Inga | Inga |
> | offerTypes/utgivare/erbjudanden/planer/konfigurationer | Inga | Inga |
> | offerTypes/utgivare/erbjudanden/planer/configs/importImage | Inga | Inga |
> | privategalleryitems | Inga | Inga |
> | privateStoreClient | Inga | Inga |
> | privateStores | Inga | Inga |
> | privateStores/erbjudanden | Inga | Inga |
> | läkemedle | Inga | Inga |
> | Utgivare | Inga | Inga |
> | Utgivare/erbjudanden | Inga | Inga |
> | Utgivare/erbjudanden/ändringar | Inga | Inga |
> | registrera | Inga | Inga |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Ja | Ja |
> | updateCommunicationPreference | Inga | Inga |

## <a name="microsoftmarketplaceordering"></a>Microsoft. MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | villkor | Inga | Inga |
> | offertypes | Inga | Inga |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Media Services | Ja | Ja |
> | Media Services/accountFilters | Inga | Inga |
> | Media Services/till gångar | Inga | Inga |
> | Media Services/assets/assetFilters | Inga | Inga |
> | Media Services/contentKeyPolicies | Inga | Inga |
> | Media Services/eventGridFilters | Inga | Inga |
> | Media Services/liveEventOperations | Inga | Inga |
> | Media Services/liveEvents | Ja | Ja |
> | Media Services/liveEvents/liveOutputs | Inga | Inga |
> | Media Services/liveOutputOperations | Inga | Inga |
> | Media Services/mediaGraphs | Inga | Inga |
> | Media Services/streamingEndpointOperations | Inga | Inga |
> | Media Services/strömnings slut punkter | Ja | Ja |
> | Media Services/streamingLocators | Inga | Inga |
> | Media Services/streamingPolicies | Inga | Inga |
> | Media Services/transformeringar | Inga | Inga |
> | Media Services/transformeringar/jobb | Inga | Inga |

## <a name="microsoftmicroservices4spring"></a>Microsoft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | appClusters | Ja | Ja |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Ja | Ja |
> | migrateprojects | Ja | Ja |
> | moveCollections | Ja | Ja |
> | samarbetsprojekt | Ja | Ja |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | holographicsBroadcastAccounts | Ja | Ja |
> | objectUnderstandingAccounts | Ja | Ja |
> | remoteRenderingAccounts | Ja | Ja |
> | spatialAnchorsAccounts | Ja | Ja |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Ja | Inga |
> | netAppAccounts / accountBackups | Inga | Inga |
> | netAppAccounts / capacityPools | Ja | Inga |
> | netAppAccounts/capacityPools/Volumes | Ja | Inga |
> | netAppAccounts/capacityPools/volym/ögonblicks bilder | Inga | Inga |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | applicationGateways | Ja | Ja |
> | applicationGatewayWebApplicationFirewallPolicies | Ja | Ja |
> | applicationSecurityGroups | Ja | Ja |
> | azureFirewallFqdnTags | Inga | Inga |
> | azureFirewalls | Ja | Inga |
> | bastionHosts | Ja | Inga |
> | bgpServiceCommunities | Inga | Inga |
> | anslutning | Ja | Ja |
> | ddosCustomPolicies | Ja | Ja |
> | ddosProtectionPlans | Ja | Ja |
> | dnsOperationStatuses | Inga | Inga |
> | dnszones | Ja | Ja |
> | dnszones/A | Inga | Inga |
> | dnszones/AAAA | Inga | Inga |
> | dnszones/alla | Inga | Inga |
> | dnszones/CAA | Inga | Inga |
> | dnszones/CNAME | Inga | Inga |
> | dnszones/MX | Inga | Inga |
> | dnszones/NS | Inga | Inga |
> | dnszones/PTR | Inga | Inga |
> | dnszones/Recordset | Inga | Inga |
> | dnszones/SOA | Inga | Inga |
> | dnszones/SRV | Inga | Inga |
> | dnszones/TXT | Inga | Inga |
> | expressRouteCircuits | Ja | Ja |
> | expressRouteCrossConnections | Ja | Ja |
> | expressRouteGateways | Ja | Ja |
> | expressRoutePorts | Ja | Ja |
> | expressRouteServiceProviders | Inga | Inga |
> | firewallPolicies | Ja | Ja |
> | frontdoors | Ja, men begränsat (se [Obs! nedan](#frontdoor)) | Ja |
> | frontdoorWebApplicationFirewallManagedRuleSets | Ja, men begränsat (se [Obs! nedan](#frontdoor)) | Inga |
> | frontdoorWebApplicationFirewallPolicies | Ja, men begränsat (se [Obs! nedan](#frontdoor)) | Ja |
> | getDnsResourceReference | Inga | Inga |
> | internalNotify | Inga | Inga |
> | Belastningsutjämnare | Ja | Ja |
> | localNetworkGateways | Ja | Ja |
> | natGateways | Ja | Ja |
> | networkIntentPolicies | Ja | Ja |
> | networkInterfaces | Ja | Ja |
> | networkProfiles | Ja | Ja |
> | networkSecurityGroups | Ja | Ja |
> | networkWatchers | Ja | Ja |
> | networkWatchers / connectionMonitors | Ja | Inga |
> | networkWatchers / flowLogs | Inga | Inga |
> | networkWatchers/linser | Ja | Inga |
> | networkWatchers / pingMeshes | Ja | Inga |
> | p2sVpnGateways | Ja | Ja |
> | privateDnsOperationStatuses | Inga | Inga |
> | privateDnsZones | Ja | Ja |
> | privateDnsZones/A | Inga | Inga |
> | privateDnsZones/AAAA | Inga | Inga |
> | privateDnsZones/alla | Inga | Inga |
> | privateDnsZones/CNAME | Inga | Inga |
> | privateDnsZones/MX | Inga | Inga |
> | privateDnsZones/PTR | Inga | Inga |
> | privateDnsZones/SOA | Inga | Inga |
> | privateDnsZones/SRV | Inga | Inga |
> | privateDnsZones/TXT | Inga | Inga |
> | privateDnsZones / virtualNetworkLinks | Ja | Ja |
> | privateEndpoints | Ja | Ja |
> | privateLinkServices | Ja | Ja |
> | publicIPAddresses | Ja | Ja |
> | publicIPPrefixes | Ja | Ja |
> | routeFilters | Ja | Ja |
> | routeTables | Ja | Ja |
> | serviceEndpointPolicies | Ja | Ja |
> | trafficManagerGeographicHierarchies | Inga | Inga |
> | trafficmanagerprofiles | Ja | Ja |
> | trafficmanagerprofiles/termiska kartor | Inga | Inga |
> | trafficManagerUserMetricsKeys | Inga | Inga |
> | virtualHubs | Ja | Ja |
> | virtualNetworkGateways | Ja | Ja |
> | virtualNetworks | Ja | Ja |
> | virtualNetworks/undernät | Inga | Inga |
> | virtualNetworkTaps | Ja | Ja |
> | virtualWans | Ja | Inga |
> | vpnGateways | Ja | Ja |
> | vpnSites | Ja | Ja |
> | webApplicationFirewallPolicies | Ja | Ja |

<a id="frontdoor" />

> [!NOTE]
> För Azures frontend-tjänst kan du använda taggar när du skapar resursen, men att uppdatera eller lägga till taggar stöds inte för närvarande.


## <a name="microsoftnotebooks"></a>Microsoft. Notebook

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | NotebookProxies | Inga | Inga |

## <a name="microsoftnotificationhubs"></a>Microsoft. NotificationHubs

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | namn områden | Ja | Inga |
> | namnrymder/notificationHubs | Ja | Inga |

## <a name="microsoftobjectstore"></a>Microsoft. ObjectStore

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | osNamespaces | Ja | Ja |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | HyperVSites | Ja | Ja |
> | ImportSites | Ja | Ja |
> | ServerSites | Ja | Ja |
> | VMwareSites | Ja | Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft. OperationalInsights

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | kluster | Ja | Ja |
> | linkTargets | Inga | Inga |
> | storageInsightConfigs | Inga | Inga |
> | arbetsytor | Ja | Ja |
> | arbets ytor/dataExports | Inga | Inga |
> | arbets ytor/data källor | Inga | Inga |
> | arbets ytor/linkedServices | Inga | Inga |
> | arbets ytor/linkedStorageAccounts | Inga | Inga |
> | arbets ytor/fråga | Inga | Inga |
> | arbets ytor/scopedPrivateLinkProxies | Inga | Inga |

## <a name="microsoftoperationsmanagement"></a>Microsoft. OperationsManagement

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | managementassociations | Inga | Inga |
> | managementconfigurations | Ja | Ja |
> | lösningar | Ja | Ja |
> | vyer | Ja | Ja |

## <a name="microsoftpeering"></a>Microsoft. peering

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | legacyPeerings | Inga | Inga |
> | peerAsns | Inga | Inga |
> | peerings | Ja | Ja |
> | peeringServiceCountries | Inga | Inga |
> | peeringServiceProviders | Inga | Inga |
> | peeringServices | Ja | Ja |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | policyEvents | Inga | Inga |
> | policyMetadata | Inga | Inga |
> | policyStates | Inga | Inga |
> | policyTrackedResources | Inga | Inga |
> | reparationer | Inga | Inga |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> |  -konsoler | Inga | Inga |
> | instrumentpaneler | Ja | Ja |
> | userSettings | Inga | Inga |

## <a name="microsoftpowerbi"></a>Microsoft. PowerBI

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft. PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | kapaciteter | Ja | Ja |

## <a name="microsoftprojectbabylon"></a>Microsoft. ProjectBabylon

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Ja | Ja |

## <a name="microsoftquantum"></a>Microsoft. Quantum

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | Arbetsytor | Ja | Ja |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | Inga | Inga |
> | valv | Ja | Ja |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | namn områden | Ja | Ja |
> | namnrymder/authorizationrules | Inga | Inga |
> | namnrymder/hybridconnections | Inga | Inga |
> | namnrymder/hybridconnections/authorizationrules | Inga | Inga |
> | namnrymder/wcfrelays | Inga | Inga |
> | namnrymder/wcfrelays/authorizationrules | Inga | Inga |

## <a name="microsoftremoteapp"></a>Microsoft. RemoteApp

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | konton | Inga | Inga |
> | Samlingar | Ja | Ja |
> | Samlingar/program | Inga | Inga |
> | Samlingar/securityprincipals | Inga | Inga |
> | templateImages | Inga | Inga |

## <a name="microsoftresourcegraph"></a>Microsoft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | skickar | Ja | Ja |
> | resourceChangeDetails | Inga | Inga |
> | resourceChanges | Inga | Inga |
> | resources | Inga | Inga |
> | resourcesHistory | Inga | Inga |
> | subscriptionsStatus | Inga | Inga |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | Inga | Inga |
> | childAvailabilityStatuses | Inga | Inga |
> | childResources | Inga | Inga |
> | emergingissues | Inga | Inga |
> | händelser | Inga | Inga |
> | impactedResources | Inga | Inga |
> | metadata | Inga | Inga |
> | meddelanden | Inga | Inga |

## <a name="microsoftresources"></a>Microsoft. Resources

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | distributioner | Ja | Inga |
> | distributioner/åtgärder | Inga | Inga |
> | deploymentScripts | Ja | Ja |
> | deploymentScripts/loggar | Inga | Inga |
> | Länkar | Inga | Inga |
> | notifyResourceJobs | Inga | Inga |
> | finansiär | Inga | Inga |
> | resourceGroups | Ja | Inga |
> | prenumerationer | Ja | Inga |
> | klienter | Inga | Inga |

## <a name="microsoftsaas"></a>Microsoft. SaaS

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | program | Ja | Ja |
> | saasresources | Inga | Inga |

## <a name="microsoftsearch"></a>Microsoft. search

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | Inga | Inga |
> | searchServices | Ja | Ja |

## <a name="microsoftsecurity"></a>Microsoft. Security

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | Inga | Inga |
> | advancedThreatProtectionSettings | Inga | Inga |
> | aviseringar | Inga | Inga |
> | allowedConnections | Inga | Inga |
> | applicationWhitelistings | Inga | Inga |
> | assessmentMetadata | Inga | Inga |
> | utvärderingar | Inga | Inga |
> | autoDismissAlertsRules | Inga | Inga |
> | automatiseringar | Ja | Ja |
> | AutoProvisioningSettings | Inga | Inga |
> | Godkännanden | Inga | Inga |
> | dataCollectionAgents | Inga | Inga |
> | deviceSecurityGroups | Inga | Inga |
> | discoveredSecuritySolutions | Inga | Inga |
> | externalSecuritySolutions | Inga | Inga |
> | InformationProtectionPolicies | Inga | Inga |
> | iotSecuritySolutions | Ja | Ja |
> | iotSecuritySolutions / analyticsModels | Inga | Inga |
> | iotSecuritySolutions / analyticsModels / aggregatedAlerts | Inga | Inga |
> | iotSecuritySolutions / analyticsModels / aggregatedRecommendations | Inga | Inga |
> | jitNetworkAccessPolicies | Inga | Inga |
> | networkData | Inga | Inga |
> | policies | Inga | Inga |
> | prissättningar | Inga | Inga |
> | regulatoryComplianceStandards | Inga | Inga |
> | regulatoryComplianceStandards / regulatoryComplianceControls | Inga | Inga |
> | regulatoryComplianceStandards / regulatoryComplianceControls / regulatoryComplianceAssessments | Inga | Inga |
> | secureScoreControlDefinitions | Inga | Inga |
> | secureScoreControls | Inga | Inga |
> | secureScores | Inga | Inga |
> | secureScores / secureScoreControls | Inga | Inga |
> | securityContacts | Inga | Inga |
> | securitySolutions | Inga | Inga |
> | securitySolutionsReferenceData | Inga | Inga |
> | securityStatuses | Inga | Inga |
> | securityStatusesSummaries | Inga | Inga |
> | serverVulnerabilityAssessments | Inga | Inga |
> | settings | Inga | Inga |
> | underbedömningar | Inga | Inga |
> | uppgifter | Inga | Inga |
> | topologier | Inga | Inga |
> | workspaceSettings | Inga | Inga |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Inga | Inga |
> | diagnosticSettingsCategories | Inga | Inga |

## <a name="microsoftsecurityinsights"></a>Microsoft. SecurityInsights

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | agg regeringar | Inga | Inga |
> | alertRules | Inga | Inga |
> | alertRuleTemplates | Inga | Inga |
> | bokmärken | Inga | Inga |
> | fall | Inga | Inga |
> | dataConnectors | Inga | Inga |
> | dataConnectorsCheckRequirements | Inga | Inga |
> | poster | Inga | Inga |
> | entityQueries | Inga | Inga |
> | incidenter | Inga | Inga |
> | officeConsents | Inga | Inga |
> | settings | Inga | Inga |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | namn områden | Ja | Ja |
> | namnrymder/authorizationrules | Inga | Inga |
> | namnrymder/disasterrecoveryconfigs | Inga | Inga |
> | namnrymder/eventgridfilters | Inga | Inga |
> | namnrymder/networkrulesets | Inga | Inga |
> | namnrymder/köer | Inga | Inga |
> | namnrymder/köer/authorizationrules | Inga | Inga |
> | namn områden/ämnen | Inga | Inga |
> | namnrymder/ämnen/authorizationrules | Inga | Inga |
> | namnrymder/ämnen/prenumerationer | Inga | Inga |
> | namn områden/ämnen/prenumerationer/regler | Inga | Inga |
> | premiumMessagingRegions | Inga | Inga |

## <a name="microsoftservicefabric"></a>Microsoft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | program | Ja | Ja |
> | kluster | Ja | Ja |
> | kluster/program | Inga | Inga |
> | containerGroups | Ja | Ja |
> | containerGroupSets | Ja | Ja |
> | edgeclusters | Ja | Ja |
> | edgeclusters/program | Inga | Inga |
> | managedclusters | Ja | Ja |
> | managedclusters/nodetypes | Inga | Inga |
> | nätet | Ja | Ja |
> | secretstores | Ja | Ja |
> | secretstores/certifikat | Inga | Inga |
> | secretstores/hemligheter | Inga | Inga |
> | volumes | Ja | Ja |

## <a name="microsoftservicefabricmesh"></a>Microsoft. ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | program | Ja | Ja |
> | containerGroups | Ja | Ja |
> | gatewayer | Ja | Ja |
> | nätet | Ja | Ja |
> | secrets | Ja | Ja |
> | volumes | Ja | Ja |

## <a name="microsoftservices"></a>Microsoft. Services

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | providerRegistrations | Inga | Inga |
> | providerRegistrations / resourceTypeRegistrations | Inga | Inga |
> | distributioner | Ja | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | SignalR | Ja | Ja |
> | SignalR/eventGridFilters | Inga | Inga |

## <a name="microsoftsiterecovery"></a>Microsoft. SiteRecovery

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Ja | Ja |

## <a name="microsoftsoftwareplan"></a>Microsoft. SoftwarePlan

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | hybridUseBenefits | Inga | Inga |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Ja | Ja |
> | program | Ja | Ja |
> | jitRequests | Ja | Ja |

## <a name="microsoftspoolservice"></a>Microsoft. SpoolService

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | registeredSubscriptions | Inga | Inga |
> | buffertar | Ja | Ja |


## <a name="microsoftsql"></a>Microsoft. SQL

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | managedInstances | Ja | Ja |
> | managedInstances/databaser | Ja (se [Anmärkning nedan](#sqlnote)) | Ja |
> | managedInstances/databaser/backupShortTermRetentionPolicies | Inga | Inga |
> | managedInstances/databaser/scheman/tabeller/kolumner/sensitivityLabels | Inga | Inga |
> | managedInstances/databaser/vulnerabilityAssessments | Inga | Inga |
> | managedInstances/databaser/vulnerabilityAssessments/regler/bas linjer | Inga | Inga |
> | managedInstances / encryptionProtector | Inga | Inga |
> | managedInstances/nycklar | Inga | Inga |
> | managedInstances / restorableDroppedDatabases / backupShortTermRetentionPolicies | Inga | Inga |
> | managedInstances / vulnerabilityAssessments | Inga | Inga |
> | brygghuvudservrar | Ja | Ja |
> | servrar/administratörer | Inga | Inga |
> | servrar/communicationLinks | Inga | Inga |
> | servrar/databaser | Ja (se [Anmärkning nedan](#sqlnote)) | Ja |
> | servrar/encryptionProtector | Inga | Inga |
> | servrar/firewallRules | Inga | Inga |
> | servrar/nycklar | Inga | Inga |
> | servrar/restorableDroppedDatabases | Inga | Inga |
> | servrar/serviceobjectives | Inga | Inga |
> | servrar/tdeCertificates | Inga | Inga |
> | virtualClusters | Inga | Inga |

<a id="sqlnote" />

> [!NOTE]
> Huvud databasen stöder inte taggar, men andra databaser, inklusive Azure SQL Data Warehouse-databaser, stöder taggar. Azure SQL Data Warehouse-databaser måste vara i aktiv (inte pausad) status.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Ja | Ja |
> | SqlVirtualMachineGroups / AvailabilityGroupListeners | Inga | Inga |
> | SqlVirtualMachines | Ja | Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Ja | Ja |
> | storageAccounts/blobServices | Inga | Inga |
> | storageAccounts/fileServices | Inga | Inga |
> | storageAccounts/queueServices | Inga | Inga |
> | storageAccounts/tjänster | Inga | Inga |
> | storageAccounts/tjänster/metricDefinitions | Inga | Inga |
> | storageAccounts/tableServices | Inga | Inga |
> | användningar | Inga | Inga |

## <a name="microsoftstoragecache"></a>Microsoft. StorageCache

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | cacheminnen | Ja | Ja |
> | cache-/storageTargets | Inga | Inga |
> | usageModels | Inga | Inga |

## <a name="microsoftstoragereplication"></a>Microsoft. StorageReplication

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | replicationGroups | Inga | Inga |

## <a name="microsoftstoragesync"></a>Microsoft. StorageSync

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Ja | Ja |
> | storageSyncServices / registeredServers | Inga | Inga |
> | storageSyncServices / syncGroups | Inga | Inga |
> | storageSyncServices / syncGroups / cloudEndpoints | Inga | Inga |
> | storageSyncServices / syncGroups / serverEndpoints | Inga | Inga |
> | storageSyncServices/arbets flöden | Inga | Inga |

## <a name="microsoftstoragesyncdev"></a>Microsoft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Ja | Ja |
> | storageSyncServices / registeredServers | Inga | Inga |
> | storageSyncServices / syncGroups | Inga | Inga |
> | storageSyncServices / syncGroups / cloudEndpoints | Inga | Inga |
> | storageSyncServices / syncGroups / serverEndpoints | Inga | Inga |
> | storageSyncServices/arbets flöden | Inga | Inga |

## <a name="microsoftstoragesyncint"></a>Microsoft. StorageSyncInt

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Ja | Ja |
> | storageSyncServices / registeredServers | Inga | Inga |
> | storageSyncServices / syncGroups | Inga | Inga |
> | storageSyncServices / syncGroups / cloudEndpoints | Inga | Inga |
> | storageSyncServices / syncGroups / serverEndpoints | Inga | Inga |
> | storageSyncServices/arbets flöden | Inga | Inga |

## <a name="microsoftstorsimple"></a>Microsoft. StorSimple

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | hantera | Ja | Ja |

## <a name="microsoftstreamanalytics"></a>Microsoft. StreamAnalytics

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | streamingjobs | Ja (se anmärkning nedan) | Ja |

> [!NOTE]
> Du kan inte lägga till en tagg när streamingjobs körs. Stoppa resursen för att lägga till en tagg.

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | avbryt | Inga | Inga |
> | CreateSubscription | Inga | Inga |
> | Aktivera | Inga | Inga |
> | byt namn | Inga | Inga |
> | SubscriptionDefinitions | Inga | Inga |
> | SubscriptionOperations | Inga | Inga |
> | prenumerationer | Inga | Inga |

## <a name="microsofttimeseriesinsights"></a>Microsoft. TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | utrymmen | Ja | Inga |
> | miljöer/accessPolicies | Inga | Inga |
> | miljöer/eventsources | Ja | Inga |
> | miljöer/referenceDataSets | Ja | Inga |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft. VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | dedicatedCloudNodes | Ja | Ja |
> | dedicatedCloudServices | Ja | Ja |
> | virtualMachines | Ja | Ja |

## <a name="microsoftvnfmanager"></a>Microsoft. VnfManager

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | devices | Ja | Ja |
> | registeredSubscriptions | Inga | Inga |
> | leverantören | Inga | Inga |
> | leverantörer/SKU: er | Inga | Inga |
> | leverantörer/vnfs | Inga | Inga |
> | virtualNetworkFunctionSkus | Inga | Inga |
> | vnfs | Ja | Ja |

## <a name="microsoftweb"></a>Microsoft. Web

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | Inga | Inga |
> | apiManagementAccounts / apiAcls | Inga | Inga |
> | apiManagementAccounts/API: er | Inga | Inga |
> | apiManagementAccounts/API/apiAcls | Inga | Inga |
> | apiManagementAccounts/API/connectionAcls | Inga | Inga |
> | apiManagementAccounts/API/anslutningar | Inga | Inga |
> | apiManagementAccounts/API/Connections/connectionAcls | Inga | Inga |
> | apiManagementAccounts/API/localizedDefinitions | Inga | Inga |
> | apiManagementAccounts / connectionAcls | Inga | Inga |
> | apiManagementAccounts/anslutningar | Inga | Inga |
> | billingMeters | Inga | Inga |
> | certifikat | Ja | Ja |
> | connectionGateways | Ja | Ja |
> | anslutning | Ja | Ja |
> | customApis | Ja | Ja |
> | deletedSites | Inga | Inga |
> | hostingEnvironments | Ja | Ja |
> | hostingEnvironments / eventGridFilters | Inga | Inga |
> | hostingEnvironments / multiRolePools | Inga | Inga |
> | hostingEnvironments / workerPools | Inga | Inga |
> | kubeEnvironments | Ja | Ja |
> | publishingUsers | Inga | Inga |
> | rekommendationer | Inga | Inga |
> | resourceHealthMetadata | Inga | Inga |
> | körningar | Inga | Inga |
> | Server grupper | Ja | Ja |
> | Server grupper/eventGridFilters | Inga | Inga |
> | webbplatser | Ja | Ja |
> | platser/konfig  | Inga | Inga |
> | platser/eventGridFilters | Inga | Inga |
> | platser/hostNameBindings | Inga | Inga |
> | platser/networkConfig | Inga | Inga |
> | platser/premieraddons | Ja | Ja |
> | platser/platser | Ja | Ja |
> | platser/platser/eventGridFilters | Inga | Inga |
> | platser/platser/hostNameBindings | Inga | Inga |
> | platser/platser/networkConfig | Inga | Inga |
> | sourceControls | Inga | Inga |
> | staticSites | Ja | Ja |
> | kontrollerar | Inga | Inga |
> | verifyHostingEnvironmentVnet | Inga | Inga |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft. WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Inga | Inga |
> | diagnosticSettingsCategories | Inga | Inga |

## <a name="microsoftwindowsiot"></a>Microsoft. WindowsIoT

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Ja | Ja |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Resurstyp | Stöder Taggar | Tagga i kostnads rapport |
> | ------------- | ----------- | ----------- |
> | delarna | Inga | Inga |
> | componentsSummary | Inga | Inga |
> | monitorInstances | Inga | Inga |
> | monitorInstancesSummary | Inga | Inga |
> | Övervakare | Inga | Inga |
> | notificationSettings | Inga | Inga |

## <a name="next-steps"></a>Nästa steg

Information om hur du använder taggar för resurser finns i [använda taggar för att ordna dina Azure-resurser](tag-resources.md).
