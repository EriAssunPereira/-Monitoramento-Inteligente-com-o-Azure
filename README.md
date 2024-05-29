# Monitoramento-Inteligente-com-o-Azure

**Projeto: Monitoramento Inteligente com o Azure**

Este projeto é focado em fornecer uma experiência prática com as ferramentas de monitoramento e insights dentro da Azure. Vamos dividir isso em módulos para facilitar a compreensão.

**Módulo 1: Azure Monitor**
O Azure Monitor é uma ferramenta que coleta, analisa e atua sobre os dados telemétricos de seus aplicativos na nuvem e no local para ajudá-lo a entender o desempenho e a disponibilidade.

```python
# Exemplo de código para consultar métricas do Azure Monitor
from azure.mgmt.monitor import MonitorManagementClient
from azure.mgmt.monitor.models import TimeInterval

monitor_client = MonitorManagementClient(credentials, subscription_id)

metrics_data = monitor_client.metrics.list(
    resource_uri='meuRecurso',
    timespan=TimeInterval(start='2024-05-01T00:00:00Z', end='2024-05-31T23:59:59Z'),
    interval='PT1H',
    metricnames='CpuPercentage',
    aggregation='Average'
)

for item in metrics_data.value:
    print(f"{item.name.value}: {item.timeseries[0].data[0].average}")
```

**Módulo 2: Azure Log Analytics**
O Azure Log Analytics é um serviço que ajuda a coletar e analisar dados gerados por recursos em sua nuvem e no local.

```python
# Exemplo de código para consultar logs com o Azure Log Analytics
from azure.loganalytics import LogAnalyticsDataClient
from azure.loganalytics.models import QueryBody

log_client = LogAnalyticsDataClient(credentials)

query_body = QueryBody(query='search * | limit 10')

response = log_client.query(workspace_id='meuWorkspace', body=query_body)

for table in response.tables:
    for row in table.rows:
        print(row)
```

**Módulo 3: Azure Application Insights**
O Azure Application Insights é uma ferramenta de monitoramento de desempenho de aplicativos que fornece insights telemétricos em tempo real.

```python
# Exemplo de código para enviar um evento de telemetria para o Application Insights
from applicationinsights import TelemetryClient

tc = TelemetryClient('meuInstrumentationKey')

tc.track_event('meuEvento', {'propriedade1': 'valor1'})
tc.flush()
```

Claro, o Azure Application Insights é uma ferramenta poderosa para a depuração de aplicativos. Aqui estão algumas maneiras de como você pode usá-lo:

**1. Coleta de Logs de Depuração**
Você pode configurar o Application Insights para coletar logs de depuração do seu aplicativo. Isso pode ser feito adicionando o SDK do Application Insights ao seu código e usando a função `TrackTrace` para enviar logs.

```python
from applicationinsights import TelemetryClient

tc = TelemetryClient('meuInstrumentationKey')

tc.track_trace('Mensagem de depuração')
tc.flush()
```

**2. Consulta de Logs**
Depois de coletar os logs, você pode usar a linguagem de consulta Kusto no portal do Azure para consultar seus logs. Por exemplo, você pode encontrar todas as mensagens de erro com a seguinte consulta:

```kusto
traces
| where severityLevel == 3
```

**3. Monitoramento de Exceções**
O Application Insights também pode rastrear exceções não tratadas em seu aplicativo. Você pode usar a função `TrackException` para enviar informações de exceção.

```python
from applicationinsights import TelemetryClient
import sys

tc = TelemetryClient('meuInstrumentationKey')

try:
    # algum código que pode lançar uma exceção
    x = 1 / 0
except Exception:
    tc.track_exception(*sys.exc_info())
    tc.flush()
```

**4. Perfis de Desempenho**
O Application Insights permite que você colete perfis de desempenho de seu aplicativo em produção. Isso pode ajudá-lo a entender onde seu aplicativo está gastando tempo e recursos.

Lembre-se, a depuração é um processo iterativo e o Application Insights é uma ferramenta poderosa que pode fornecer insights valiosos sobre o comportamento do seu aplicativo.
