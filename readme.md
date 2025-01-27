该模块主要是为了前端方便使用 eolinker 上的接口，自动生成可直接使用的 api 方法以及对应的参数类型。当然需要在 eolinker 将每个接口的参数以及描述按规范设置好。

## 下载

```sh
npm install generateeolinkerapi -D
```

或者

```sh
yarn add generateeolinkerapi -D
```

## 开始

cli 命令为

1. 生成 api：**generateApi**

2. 更新 api: **generateApi update** 可快捷选择历史生成的 api 模块，储存上限 100

> 可在 script 添加"api": "generateApi"方便使用，然后执行 npm run api 即可，当然也可以直接执行 generateApi

## 配置文件

在根目录下创建 generateApi.config.js，如下为默认配置

```js
module.exports = {
  username: "***", //eolinker登录名
  password: "**************************", //加密后的密码串，自行控制台查看登录接口参数
  projectId: 0, // 项目id - 支持选择项目，默认为0；如果传入具体id。则直接进入选择模块
  distDir: "./src/services", //输出目录，最好保证该目录已存在
  distFileName: "autoGenerate", //输出文件名，如果distType是inner模式，则忽略改参数
  domain: "https://eolinker.yidejia.com", //eolinker服务器域名
  distType: "inner" | "outer",
  //当使用inner模式，将根据接口的分组名创建api定义文件，中文则转成拼音 ； outer则创建固定名称的定义文件，默认为outer
  isRestfulApi: false, // 默认为false，如果true，则将在接口名上添加请求方式的前缀
};
```

## 输出样例

### Api 方法

> request 为 自定义的 axios 实例

```ts
import request from "./request";
/**
 * 删除
 * https://eolinker.yidejia.com/#/home/project/inside/api/detail?groupID=4900&apiID=22771&projectName=%E3%80%90golang%E3%80%91%E5%9F%8B%E7%82%B9%E7%B3%BB%E7%BB%9F&projectID=224
 */
export async function projectDelete(
  params: API.ProjectDeleteParams,
  project_id: string
): Promise<API.ProjectDeleteResponse> {
  return request.post(`/project/delete/${project_id}`, params);
}
/**
 * 修改
 * https://eolinker.yidejia.com/#/home/project/inside/api/detail?groupID=4900&apiID=22770&projectName=%E3%80%90golang%E3%80%91%E5%9F%8B%E7%82%B9%E7%B3%BB%E7%BB%9F&projectID=224
 */
export async function projectUpdate(
  params: API.ProjectUpdateParams,
  project_id: string
): Promise<API.ProjectUpdateResponse> {
  return request.post(`/project/update/${project_id}`, params);
}
/**
 * 列表
 * https://eolinker.yidejia.com/#/home/project/inside/api/detail?groupID=4900&apiID=22757&projectName=%E3%80%90golang%E3%80%91%E5%9F%8B%E7%82%B9%E7%B3%BB%E7%BB%9F&projectID=224
 */
export async function projectList(
  params: API.ProjectListParams
): Promise<API.ProjectListResponse> {
  return request.get(`/project/list`, { params });
}
/**
 * 添加
 * https://eolinker.yidejia.com/#/home/project/inside/api/detail?groupID=4900&apiID=22758&projectName=%E3%80%90golang%E3%80%91%E5%9F%8B%E7%82%B9%E7%B3%BB%E7%BB%9F&projectID=224
 */
export async function projectAdd(
  params: API.ProjectAddParams
): Promise<API.ProjectAddResponse> {
  return request.post(`/project/add`, params);
}
/**
 * 获取员工列表
 * https://eolinker.yidejia.com/#/home/project/inside/api/detail?groupID=4900&apiID=23024&projectName=%E3%80%90golang%E3%80%91%E5%9F%8B%E7%82%B9%E7%B3%BB%E7%BB%9F&projectID=224
 */
export async function staffList(
  params: API.StaffListParams
): Promise<API.StaffListResponse> {
  return request.get(`/staff/list`, { params });
}
```

### 参数类型定义

```ts
declare namespace API {
  type ProjectDeleteParams = {
    /** 项目物理ID */
    id: number;
  };
  type ProjectDeleteResponse = any;
  type ProjectUpdateParams = {
    /** 项目物理ID */
    id: number;
    /** 名称 */
    project_name?: string;
    /** 描述 */
    description?: string;
    /** 平台 */
    platform?:
      | 1 /** PC */
      | 2 /** Android */
      | 3 /** IOS */
      | 4 /** H5 */
      | 0 /** 全部 */
      | 5 /** 小程序 */;
    /** 是否启用 */
    is_valid?: 1 /** 启用 */ | 0 /** 暂停 */;
    /** 管理员集合get,post：manager_staff_ids=1&manager_staff_ids=2，json：[] */
    manager_staff_ids?: (number | string)[];
  };
  type ProjectUpdateResponse = any;
  type ProjectListParams = {
    /** 是否启用 */
    is_valid?: 1 /** 启用 */ | 0 /** 暂停 */;
    /** 搜索关键字 */
    keyword?: string;
    /** 平台 */
    platform?:
      | 1 /** PC */
      | 2 /** Android */
      | 3 /** IOS */
      | 4 /** H5 */
      | 0 /** 全部 */
      | 5 /** 小程序 */;
    /** 页码 */
    page?: number;
    /** 条数 */
    per_page?: number;
    /** 返回类型 */
    resp_type?: "list" /** 列表 */ | "table" /** 表格 */;
  };
  type ProjectListResponse = {
    total: string;
    table_header: {
      group_id: string;
      group_name: string;
      columns: {
        id: string;
        order: string;
        show_title: string;
        column: string;
      }[];
    };
    data: {
      id: number;
      project_name: string;
      project_id: string;
      platform: string;
      platform_str: string;
      is_valid: number;
      description: string;
      created_at: string;
      manager: {
        staff_id: string;
        staff_name: string;
      }[];
    }[];
  };
  type ProjectAddParams = {
    /** 标识 */
    project_id: string;
    /** 名称 */
    project_name: string;
    /** 描述 */
    description?: string;
    /** 平台 */
    platform:
      | 1 /** PC */
      | 2 /** Android */
      | 3 /** IOS */
      | 4 /** H5 */
      | 0 /** 全部 */
      | 5 /** 小程序 */;
    /** 管理员ids集合 get,post：manager_staff_ids=1&manager_staff_ids=2，json：[] */
    manager_staff_ids?: (number | string)[];
  };
  type ProjectAddResponse = any;
  type StaffListParams = {
    /** 关键字 */
    keyword: string;
    /** 页码 */
    page?: number;
    /** 条数 */
    per_page?: number;
  };
  type StaffListResponse = {
    total: string;
    data: {
      staff_id: string;
      staff_name: string;
    }[];
  };
}
```

## 注意事项

1. 在获取模块列表后，需要选择模块进行 api 方法的生成，此步骤支持模糊查询
2. eolinker 服务不一定稳定，因此接口请求时间可快可慢，耐心等待即可，如果失败了，请检查账号密码和项目 id 是否正确
3. 建议将输出文件添加 eslint 忽略
4. distType 若是 inner 模式，在选择接口模块的时候，建议选择子级模块，避免产生冲突
5. 生成的 api 方法的返回类型，已提取了 axios 返回对象的 data 一层，需要注意 axios 的拦截处理

## 联系方式

邮箱：960492612@qq.com

伊的家云聊号：201109
