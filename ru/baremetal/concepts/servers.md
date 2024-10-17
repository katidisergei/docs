# Серверы

Сервер {{ baremetal-name }} — это физический сервер готовой [конфигурации](./server-configurations.md), подключенный к [сети](./network.md) двумя сетевыми интерфейсами с пропускной способностью 1 или 10 Гбит/с. Аппаратные и сетевые ресурсы сервера физически изолированы и доступны только арендовавшему их пользователю. Сервер предоставляется в аренду без доступа к настройкам BIOS.

## Аренда серверов {#server-lease}

Сервер можно арендовать на срок 1, 3, 6 месяцев или 1 год. Конфигурации, доступные для аренды в каждом пуле, можно посмотреть при заказе сервера в консоли управления. 

## Доступ на сервер {#server-access}

Управлять сервером можно с помощью KVM-консоли, которая доступна в [консоли управления]({{ link-console-main }}).

## Пулы серверов {#server-pools}

Пулы — инфраструктурно обособленные модули ЦОД, в которых физически размещаются сервера. Пулы распределены по [зонам доступности](../../overview/concepts/geo-scope.md), находящимся в [регионах](../../overview/concepts/region.md). В настоящее время сервис {{ baremetal-full-name }} доступен только в регионе Россия.

| Регион        | Зона доступности | Пулы                               |
|---------------|------------------|------------------------------------|
| `{{ region-id }}` | `{{ region-id }}-m`  | `{{ region-id }}-m3`<br>`{{ region-id }}-m4` |