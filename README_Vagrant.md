# Инструкция по настройке среды разработки с Vagrant (Ubuntu)

## Описание

Этот Vagrantfile создает готовую среду разработки для проекта управления Git-репозиториями на хостингах. Включает все необходимые инструменты из файла [Необходимое_ПО.md](Необходимое_ПО.md).

## Предварительные требования

### На хост-машине должно быть установлено:
- **Vagrant** (версия 2.2.0 или выше)
- **VirtualBox** (версия 6.0 или выше)
- **Git** (для клонирования репозитория)

### Минимальные требования к хост-системе:
- **RAM**: 16GB (рекомендуется 32GB)
- **Процессор**: 4 ядра (рекомендуется 8 ядер)
- **Свободное место**: 50GB
- **Интернет**: стабильное подключение для загрузки образов

## Виртуальная машина Ubuntu 22.04

### Характеристики:
- **IP**: 192.168.56.10
- **RAM**: 8GB
- **CPU**: 4 ядра
- **Hostname**: git-repo-manager-dev

### Установленное ПО:
- **.NET 8 SDK** - основной фреймворк разработки
- **Visual Studio Code** - IDE с расширениями
- **Git + Git Flow** - система контроля версий
- **Postman** - тестирование API
- **Node.js** - для дополнительных инструментов
- **Python 3** - для скриптов
- **AppImage tools** - для сборки пакетов
- **Avalonia Templates** - шаблоны проектов

### Расширения VS Code:
- C# Dev Kit
- Avalonia XAML
- GitLens
- JSON Tools
- NuGet Package Manager

## Команды для работы с Vagrant

### Запуск машины
```bash
vagrant up
```

### Подключение к машине
```bash
vagrant ssh
```

### Остановка машины
```bash
vagrant halt
```

### Удаление машины
```bash
vagrant destroy
```

### Перезапуск с переустановкой ПО
```bash
vagrant reload --provision
```

### Проверка статуса
```bash
vagrant status
```

## Работа с проектом

### 1. Подключение и переход в рабочую папку
```bash
vagrant ssh
cd /home/vagrant/dev
```

### 2. Создание проекта Avalonia
```bash
dotnet new avalonia.mvvm -n GitRepositoryManager
cd GitRepositoryManager
dotnet run
```

### 3. Установка дополнительных пакетов
```bash
dotnet add package Avalonia
dotnet add package Avalonia.Desktop
dotnet add package Octokit
dotnet add package Newtonsoft.Json
```

### 4. Создание тестового проекта
```bash
dotnet new xunit -n GitRepositoryManager.Tests
cd GitRepositoryManager.Tests
dotnet add reference ../GitRepositoryManager/GitRepositoryManager.csproj
```

### 5. Запуск VS Code
```bash
code .
```

## Настройка Git

Git уже настроен с базовыми параметрами:
- Username: Developer
- Email: developer@example.com

Для изменения настроек:
```bash
git config --global user.name "Ваше имя"
git config --global user.email "ваш@email.com"
```

## Полезные алиасы

В системе созданы алиасы для удобства:
```bash
dev          # Переход в рабочую папку
dotnet-run   # Запуск проекта
dotnet-build # Сборка проекта
dotnet-test  # Запуск тестов
```

## Устранение неполадок

### Проблемы с запуском
1. Проверьте версии Vagrant и VirtualBox
2. Убедитесь, что включена виртуализация в BIOS
3. Проверьте свободное место на диске

### Проблемы с установкой ПО
1. Переустановите ПО: `vagrant provision`
2. Проверьте логи: `vagrant up --debug`

### Нехватка ресурсов
Уменьшите настройки в Vagrantfile:
```ruby
vb.memory = 4096  # 4GB вместо 8GB
vb.cpus = 2       # 2 ядра вместо 4
```

### Проблемы с сетью
```bash
# Проверка SSH подключения
vagrant ssh-config

# Пересоздание машины
vagrant destroy && vagrant up
```

## Мониторинг и управление

### Проверка ресурсов
```bash
htop        # Мониторинг процессов
iotop       # Мониторинг дисковых операций
df -h       # Использование диска
free -h     # Использование памяти
```

### Проверка установленного ПО
```bash
dotnet --version
git --version
node --version
python3 --version
code --version
```

### Очистка системы
```bash
sudo apt-get autoremove -y
sudo apt-get autoclean
sudo apt-get clean
```

## Быстрый старт

1. **Клонируйте репозиторий** на хост-машине
2. **Запустите Vagrant**: `vagrant up`
3. **Подключитесь**: `vagrant ssh`
4. **Перейдите в рабочую папку**: `cd /home/vagrant/dev`
5. **Создайте проект**: `dotnet new avalonia.mvvm -n GitRepositoryManager`
6. **Запустите VS Code**: `code .`
7. **Начните разработку!**

## Поддержка

При возникновении проблем:
1. Проверьте логи Vagrant: `vagrant up --debug`
2. Убедитесь в соответствии системным требованиям
3. Попробуйте пересоздать машину: `vagrant destroy && vagrant up`
4. Проверьте версии Vagrant и VirtualBox