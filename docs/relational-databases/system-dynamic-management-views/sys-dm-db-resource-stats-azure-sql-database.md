---
title: "sys.dm_db_resource_stats (Azure SQL データベース) |Microsoft ドキュメント"
ms.custom: 
ms.date: 03/16/2016
ms.prod: 
ms.prod_service: sql-database
ms.reviewer: 
ms.service: sql-database
ms.component: dmv's
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs: TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
caps.latest.revision: "11"
author: CarlRabeler
ms.author: carlrab
manager: jhubbard
ms.workload: On Demand
ms.openlocfilehash: 41ae9665de08068d1bfa3ed919bbf43f55151a7c
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2017
---
# <a name="sysdmdbresourcestats-azure-sql-database"></a>sys.dm_db_resource_stats (Azure SQL データベース)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  CPU、I/O、およびメモリの消費量を返す、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]データベース。 1 つの行は、データベース内にアクティビティがない場合でも、15 秒ごとに存在します。 1 時間では、履歴データは維持されます。  
  
|列|データ型|Description|  
|-------------|---------------|-----------------|  
|end_time|**datetime**|UTC 時刻では、現在のレポートの間隔の終了を示します。|  
|avg_cpu_percent|**10 進数 (5,2)**|サービス層の上限に対するパーセンテージで示した、平均コンピューティング使用率。|  
|avg_data_io_percent|**10 進数 (5,2)**|データ サービス層の限度のパーセンテージで I/O の使用率は平均です。|  
|avg_log_write_percent|**10 進数 (5,2)**|サービス層の上限に対するパーセンテージで示した、平均書き込みリソース使用率。|  
|avg_memory_usage_percent|**10 進数 (5,2)**|サービス層の上限に対するパーセンテージで示した、平均メモリ使用率。<br /><br /> これには、インメモリ OLTP オブジェクトの格納に使用されるメモリが含まれます。|  
|xtp_storage_percent|**10 進数 (5,2)**|記憶域使用率、インメモリ OLTP のサービス層の制限の割合で表した (したレポート期間の末尾)。 これには、次のインメモリ OLTP オブジェクトの格納に使用されるメモリが含まれます。 メモリ最適化テーブル、インデックス、およびテーブル変数。 ALTER TABLE 操作を処理するために使用されるメモリも含まれています。<br /><br /> インメモリ OLTP では、データベースで使用されていない場合は、0 を返します。|  
|max_worker_percent|**10 進数 (5,2)**|データベースのサービス層の制限の割合の最大同時実行ワーカー (要求)。|  
|max_session_percent|**10 進数 (5,2)**|データベースのサービス層の制限の割合の最大同時セッション数。|  
|dtu_limit|**int**|現在データベースの最大 DTU このデータベースの設定この間隔中にします。|  
  
> [!TIP]  
>  これらの制限とサービス層に関する詳細コンテキストは、トピックを参照して[サービス階層](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)と[サービス層の機能と制限](https://azure.microsoft.com/documentation/articles/sql-database-performance-guidance/)です。  
  
## <a name="permissions"></a>Permissions  
 このビューには、VIEW DATABASE STATE 権限が必要です。  
  
## <a name="remarks"></a>解説  
 によって返されるデータ**sys.dm_db_resource_stats**は Basic、Standard、および Premium データベースを実行しているサービス階層/パフォーマンス レベルの DTU の制限を許可される最大のパーセンテージで表されます。 Web およびビジネス層では、これらの数字は Standard S2 パフォーマンス層に関するパーセンテージを示しています。 たとえば、avg_cpu_percent に 70% が返された場合は、Web または Business データベースに対して実行する場合は、S2 層の制限の 70% が示します。 さらに、Web およびビジネス層では、パーセンテージが 100% を超えた数値を反映している場合があります。これも、S2 層の限度に基づいています。  
  
 データベースは、最後の 60 分内の別のサーバーにフェールオーバーしたが、ビューはそのフェールオーバー以降、プライマリ データベースがされた時間のデータを返すのみです。  
  
 このデータの詳細度の低いビューでは、使用**sys.resource_stats**カタログ ビューの**マスター**データベース。 このビューでは、5 分ごとにデータをキャプチャし、履歴データを 14 日間保持します。  詳細については、次を参照してください。 [sys.resource_stats &#40;です。Azure SQL データベース &#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md).  
  
 データベースが、柔軟なプールのメンバーである場合は、割合の値として表されるリソースの統計情報は、柔軟なプールの構成で設定されているデータベースの最大の DTU 制限の割合として表されます。  
  
## <a name="example"></a>例  
  
> [!NOTE]  
>  Web およびビジネス層では、これらの数字は Standard S2 パフォーマンス層に関するパーセンテージを示しています。 たとえば、avg_cpu_percent に 70% が返された場合は、Web または Business データベースに対して実行する場合は、S2 層の制限の 70% が示します。 さらに、Web およびビジネス層では、パーセンテージが 100% を超えた数値を反映している場合があります。これも、S2 層の限度に基づいています。  
  
 次の例では、現在接続しているデータベースの最新の時間によって並べ替えられてリソース使用率のデータを返します。  
  
```  
SELECT * FROM sys.dm_db_resource_stats ORDER BY end_time DESC;  
  
```  
  
 次の例では、過去の 1 時間以上のユーザー データベースのパフォーマンス レベルで許可された最大の DTU の数の割合の観点から、平均 DTU 消費を識別します。 一貫性のあるごとに 100% 近くには、この割合として、パフォーマンス レベルを大きくしてください。  
  
```  
SELECT end_time,   
  (SELECT Max(v)    
   FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS    
   value(v)) AS [avg_DTU_percent]   
FROM sys.dm_db_resource_stats;  
  
```  
  
 次の例では、CPU の割合、データおよびログ、I/O の平均値と最大の値と、過去 1 時間のメモリ消費量を返します。  
  
```  
SELECT    
    AVG(avg_cpu_percent) AS 'Average CPU Utilization In Percent',   
    MAX(avg_cpu_percent) AS 'Maximum CPU Utilization In Percent',   
    AVG(avg_data_io_percent) AS 'Average Data IO In Percent',   
    MAX(avg_data_io_percent) AS 'Maximum Data IO In Percent',   
    AVG(avg_log_write_percent) AS 'Average Log Write Utilization In Percent',   
    MAX(avg_log_write_percent) AS 'Maximum Log Write Utilization In Percent',   
    AVG(avg_memory_usage_percent) AS 'Average Memory Usage In Percent',   
    MAX(avg_memory_usage_percent) AS 'Maximum Memory Usage In Percent'   
FROM sys.dm_db_resource_stats;  
  
```  
  
## <a name="see-also"></a>参照  
 [sys.resource_stats &#40;です。Azure SQL データベース &#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [サービス層](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/)   
 [サービス層の機能および制限](https://azure.microsoft.com/documentation/articles/sql-database-performance-guidance/)  
  
  
