# Домашнее задание к занятию "7.2. Облачные провайдеры и синтаксис Terraform."


## Задача 1 (Вариант с Yandex.Cloud). Регистрация в ЯО и знакомство с основами (необязательно, но крайне желательно).

Поскольку некоторые из предыдщих заданий мы уже выполняли в yandex.cloud, то аккаунт уже имеется. Внесем 
авторизационный токен и id облака в переменные окружения. 
```buildoutcfg
export TF_VAR_yc_token=<OATH Token>
export TF_VAR_yc_cloud_id=<cloud_id>
```
Остальные переменные укажем в vars.tf:

```
variable "yc_token" {
   default = "" #оставим пустым, поскольку внесли в env
}

variable "yc_cloud_id" {
  default = "" #оставим пустым, поскольку внесли в env
}

variable "yc_folder_id" {
  default = "<folder_id>" #возьмем из GUI
}

variable "yc_region" {
  default = "ru-central1-a"
}
```
Настроим провайдер в файле main.tf:

```buildoutcfg
terraform {
  required_providers {
    yandex = {
      source  = "yandex-cloud/yandex"
      version = "0.61.0"
    }
  }
}

# Provider
provider "yandex" {
  token     = "${var.yc_token}"
  cloud_id  = "${var.yc_cloud_id}"
  folder_id = "${var.yc_folder_id}"
  zone      = "${var.yc_region}"
}

```
## Задача 2. Создание aws ec2 или yandex_compute_instance через терраформ. 

Чтобы создать instance через терраформ решил развернуть проект, рассмотренный на лекции. Для этого склонировал 
репозиторий к себе, внес изменения в конфигурационные файлы, в том числе в ansible playbook (добавил паузу в 2 мин и 
повысил привилегии в некоторых task) и Makefile (добавил в начало строку SHELL := /bin/bash, иначе make выполнялся 
через /bin/sh, который не знает 
команду source). Добился 
разворачивания с первого раза.

Вывод комманды terraform plan:
```buildoutcfg
Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # module.news.yandex_compute_instance.instance will be created
  + resource "yandex_compute_instance" "instance" {
      + created_at                = (known after apply)
      + description               = "News App Demo"
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = "news"
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                centos:ssh-rsa <deleted>
            EOT
        }
      + name                      = "news"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v2"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = (known after apply)

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + description = (known after apply)
              + image_id    = "fd8baskja1t8rcjt19ma"
              + name        = (known after apply)
              + size        = 20
              + snapshot_id = (known after apply)
              + type        = "network-ssd"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = "e9bkk5r7n95ahpt07is8"
        }

      + placement_policy {
          + placement_group_id = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 2
          + memory        = 2
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these
actions if you run "terraform apply" now.
```

Скрин с запущенной ВМ:
![](assets/scr01.jpg)

Скрин с работающим приложением:
![](assets/scr02.jpg)