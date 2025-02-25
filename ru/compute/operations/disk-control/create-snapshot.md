# Создать снимок диска

_Снимок диска_ — это копия файловой системы диска на определенный момент времени.

{% include [snapshot-disk-types](../../../_includes/compute/snapshot-disk-types.md) %}

## Подготовка {#prepare}

Снимок диска содержит только те данные, которые были записаны на диск в момент создания снимка. Если диск подключен к работающей виртуальной машине, то кеш приложений и операционной системы не попадет в снимок.

Чтобы обеспечить целостность данных снимка:

**Для Linux-систем:**

1. Остановите все операции записи на диск в приложениях.

1. Запишите кэш операционной системы на диск:

    ```bash
    sync
    ```

1. Заморозьте файловую систему:

    ```bash
    sudo fsfreeze --freeze <точка_монтирования>
    ```
    Где `--freeze` — параметр для заморозки файловой системы. Вместо `<точка_монтирования>` укажите каталог, к которому подключена файловая система. Например: `/mnt/vdc2`.

1. Создайте снимок по инструкции [ниже](#create).

1. Разморозьте файловую систему:

    ```bash
    sudo fsfreeze --unfreeze <точка_монтирования>
    ```
    Где `--unfreeze` — параметр для разморозки файловой системы. Вместо `<точка_монтирования>` укажите каталог, к которому подключена файловая система. Например: `/mnt/vdc2`.

**Для остальных систем:**

1. Остановите виртуальную машину (см. раздел [{#T}](../vm-control/vm-stop-and-start.md#stop)).
1. Дождитесь, когда статус машины изменится на `STOPPED`.

## Создание снимка {#create}

Чтобы создать снимок диска:

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) выберите каталог, в котором находится диск.
  1. Выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_compute }}**.
  1. На панели слева выберите ![image](../../../_assets/compute/disks-pic.svg) **{{ ui-key.yacloud.compute.switch_disks }}**.
  1. В строке с диском нажмите значок ![image](../../../_assets/horizontal-ellipsis.svg) и выберите **{{ ui-key.yacloud.compute.disks.button_action-snapshot }}**.
  1. Введите имя снимка:

      {% include [name-format](../../../_includes/name-format.md) %}

  1. Если требуется, укажите произвольное текстовое описание снимка.
  1. Нажмите кнопку **{{ ui-key.yacloud.common.create }}**.

- CLI

  {% include [cli-install](../../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команд CLI для создания снимков:

      ```bash
      yc compute snapshot create --help
      ```

  1. Выберите диск, снимок которого необходимо создать. Получить список дисков в каталоге по умолчанию можно с помощью команды:

      {% include [compute-disk-list](../../../_includes/compute/disk-list.md) %}

  1. Создайте снимок в каталоге по умолчанию:

      ```bash
      yc compute snapshot create \
        --name first-snapshot \
        --description "my first snapshot via CLI" \
        --disk-id fhm4aq4hvq5g3nepvt9b
      ```

      В результате будет создан снимок диска с именем `first-snapshot` и описанием `my first snapshot via CLI`.

      {% include [name-format](../../../_includes/name-format.md) %}

- API

  1. Получите список дисков с помощью метода REST API [list](../../api-ref/Disk/list.md) для ресурса [Disk](../../api-ref/Disk/index.md) или вызова gRPC API [DiskService/List](../../api-ref/grpc/disk_service.md#List).
  1. Создайте снимок с помощью метода REST API [create](../../api-ref/Snapshot/create.md) для ресурса [Snapshot](../../api-ref/Snapshot/index.md) или вызова gRPC API [SnapshotService/Create](../../api-ref/grpc/snapshot_service.md#Create).

- {{ TF }}

  Если у вас ещё нет {{ TF }}, [установите его и настройте провайдер {{ yandex-cloud }}](../../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform).  

  1. Опишите в конфигурационном файле параметры ресурса `yandex_compute_snapshot`.

     Пример структуры конфигурационного файла:
     
     ```hcl
     resource "yandex_compute_snapshot" "snapshot-1" {
       name           = "disk-snapshot"
       source_disk_id = "<идентификатор диска>"
     }
     ```

     Более подробную информацию о ресурсах, которые вы можете создать с помощью {{ TF }}, см. в [документации провайдера]({{ tf-provider-link }}/).

  1. Проверьте корректность конфигурационных файлов.

     1. В командной строке перейдите в папку, где вы создали конфигурационный файл.
     1. Выполните проверку с помощью команды:

        ```bash
        terraform plan
        ```

     Если конфигурация описана верно, в терминале отобразится список создаваемых ресурсов и их параметров. Если в конфигурации есть ошибки, {{ TF }} на них укажет. 

  1. Разверните облачные ресурсы.

     1. Если в конфигурации нет ошибок, выполните команду:

        ```bash
        terraform apply
        ```

     1. Подтвердите создание ресурсов.

     После этого в указанном каталоге будут созданы все требуемые ресурсы. Проверить появление ресурсов и их настройки можно в [консоли управления]({{ link-console-main }}).

{% endlist %}

Создание снимка выполняется асинхронно. Снимок создается сразу после команды создания и получает статус `Creating`. С этого момента можно возобновить запись на диск, операции с диском не повлияют на данные в снимке.

Когда создание снимка завершено, статус снимка изменится на `Ready`. С этого момента снимок можно использовать для создания образов, наполнения дисков и т. п.

{% note alert %}

{% include [include](../../../_includes/compute/duplicated-uuid-note.md) %}

Чтобы этого не произошло, подключите диск к ВМ и поменяйте все дублирующиеся UUID. Подробнее в [инструкции про подключение диска](../vm-control/vm-attach-disk.md).

{% endnote %}


#### См. также {#see-also}

* [{#T}](../snapshot-control/create-schedule.md)
* [{#T}](../disk-create/from-snapshot.md)
