# Модуль 4.2 Github Actions

Целью данного модуля является построение простого сборочного конвеера с использованием [Github Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions). 

## Предварительные требования

Предполагается, что Вы:

1. Выполнили задания по модулю [Docker](https://github.com/digital-academy-devops/docker-module) и будете использовать результаты в качестве приложения для реализации CI конвеера.


## Задание
1. Добавьте [workflow](https://docs.github.com/en/actions/using-workflows) для сборки [образов тестового приложения](https://github.com/digital-academy-devops/docker-module#%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5):
   1. builder образа;
   1. release образа.
  Для сборки образов используйте [официальный action](https://github.com/marketplace/actions/build-and-push-docker-images) (описан в [документации Docker](https://docs.docker.com/build/ci/github-actions/)).
1. Workflow должен запускаться (поле [on](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on), [описание модели запуска workflow](https://docs.github.com/en/actions/using-workflows/triggering-a-workflow)) при добавлении изменений в любую ветку репозитория (событие [push](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#push)), а также иметь возможность ручного запуска (событие [workflow_dispatch](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch)).
1. Тег образов определяется следующим образом:
   - при автоматическом запуске, в качестве тега используется имя ветки/тега для которого запустилась сборка;
   - при ручном запуске, задаётся пользователем либо используется значение по умолчанию - `latest`.

### Дополнительно
1. Используйте [matrix](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) для сборки обоих образов с использованием одного набора шагов.
1. Добавьте [собственный action](https://docs.github.com/en/actions/creating-actions/about-custom-actions) типа [composite](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) для запуска сборки образов, который будет использоваться в workflow вместо официального.
1. Зарегистрируйтесь на [Dockerhub](https://hub.docker.com/) и добавьте в пайплайн push образа при сборке из релизных тегов (описано в [документации Docker](https://docs.docker.com/build/ci/github-actions/)).

## Пример выполнения задания
Пример реализации workflow для тестового приложения - https://github.com/digital-academy-devops/docker-example/pull/6
Включает в себя:
- Workflow для сборки тестового приложения - [.github/workflows/ci.yaml](https://github.com/digital-academy-devops/docker-example/pull/6/files#diff-944291df2c9c06359d37cc8833d182d705c9e8c3108e7cfe132d61a06e9133dd).
  - использует [официальный action](https://github.com/marketplace/actions/build-and-push-docker-images).
  - использует [matrix](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) для сборки образов из разных стадий Dockerfile.
- Composite action для запуска сборки образа докер - [.github/actions/build_image/action.yaml](https://github.com/digital-academy-devops/docker-example/pull/6/files#diff-8bcb50609a141d81d1813d30aa27a0ac9350e213776512048de193e358d37e37).
- Workflow для матричной сборки всех комбинаций образов и базовых образов - [.github/workflows/build_all.yaml](https://github.com/digital-academy-devops/docker-example/pull/6/files#diff-510aeea3ff7c224a6b784d120ef8d50dd7ca8ab274ed541a9ad17436492f5b11).
  - использует собственную реализацию composite action для сборки образов. 

