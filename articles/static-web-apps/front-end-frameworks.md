---
title: 使用 Azure 靜態 Web Apps 預覽版設定前端架構
description: Azure 靜態 Web Apps 所需的熱門前端架構設定
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 07/18/2020
ms.author: cshoe
ms.openlocfilehash: 14564b0591ef0146131b3f9324556b613e25daac
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97901227"
---
# <a name="configure-front-end-frameworks-and-libraries-with-azure-static-web-apps-preview"></a>使用 Azure 靜態 Web Apps 預覽版設定前端架構和程式庫

Azure 靜態 Web Apps 要求您在前端架構或程式庫的 [組建設定檔](github-actions-workflow.md) 中具有適當的設定值。

## <a name="configuration"></a>組態

下表列出一系列 framework 和程式庫<sup>1</sup>的設定。

下列專案會說明資料表資料行的意圖：

- **輸出位置**：列出的值 `output_location` ，這是 [應用程式檔的內建版本資料夾](github-actions-workflow.md#build-and-deploy)。

- **自訂群組建命令**：當 framework 需要與或不同的命令時 `npm run build` `npm run azure:build` ，您可以定義 [自訂群組建命令](github-actions-workflow.md#custom-build-commands)。

| 架構 | 應用程式成品位置 | 自訂群組建命令 |
|--|--|--|
| [Alpine.js](https://github.com/alpinejs/alpine/) | `/` | n/a <sup>2</sup> |
| [Angular](https://angular.io/) | `dist/<APP_NAME>` | `npm run build -- --prod` |
| [角度通用](https://angular.io/guide/universal) | `dist/<APP_NAME>/browser` | `npm run prerender` |
| [Aurelia](https://aurelia.io/) | `dist` | n/a |
| [Backbone.js](https://backbonejs.org/) | `/` | n/a |
| [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor) | `wwwroot` | n/a |
| [Ember](https://emberjs.com/) | `dist` | n/a |
| [Flutter](https://flutter.dev/) | `build/web` | `flutter build web` |
| [Framework7](https://framework7.io/) | `www` | `npm run build-prod` |
| [一絲](https://glimmerjs.com/) | `dist` | n/a |
| [HTML](https://developer.mozilla.org/docs/Web/HTML) | `/` | n/a |
| [Hyperapp](https://hyperapp.dev/) | `/` | n/a |
| [JavaScript](https://developer.mozilla.org/docs/Web/javascript) | `/` | n/a |
| [jQuery](https://jquery.com/) | `/` | n/a |
| [KnockoutJS](https://knockoutjs.com/) | `dist` | n/a |
| [LitElement](https://lit-element.polymer-project.org/) | `dist` | n/a |
| [馬可](https://markojs.com/) | `public` | n/a |
| [流星](https://www.meteor.com/) | `bundle` | n/a |
| [Mithril](https://mithril.js.org/) | `dist` | n/a |
| [聚合物](https://www.polymer-project.org/) | `build/default` | n/a |
| [Preact](https://preactjs.com/) | `build` | n/a |
| [React](https://reactjs.org/) | `build` | n/a |
| [樣板](https://stenciljs.com/) | `www` | n/a |
| [Svelte](https://svelte.dev/) | `public` | n/a |
| [Three.js](https://threejs.org/) | `/` | n/a |
| [TypeScript](https://www.typescriptlang.org/) | `dist` | n/a |
| [Vue.js](https://vuejs.org/) | `dist` | n/a |

<sup>1</sup> 上表不是適用于 Azure 靜態 Web Apps 的完整架構和程式庫清單。

<sup>2</sup> 不適用

## <a name="next-steps"></a>後續步驟

- [組建和工作流程設定](github-actions-workflow.md)
