# 电竞陪玩护航系统 - EasyAdmin二次开发规范

## 概述

本文档详细描述电竞陪玩护航系统管理后台基于EasyAdmin框架的二次开发规范，包括项目目录结构、代码编写规范、扩展开发指南、权限管理规范等内容，旨在帮助开发团队快速、规范地进行后台功能开发。

EasyAdmin是一款基于ThinkPHP8.1框架开发的极速后台开发框架，提供了完善的用户管理、权限管理、菜单管理、日志管理等功能，本项目将基于EasyAdmin进行二次开发，实现电竞陪玩平台的管理后台功能。

---

## 一、项目结构

### 1.1 整体目录结构

```
admin/                           # 管理后台根目录
├── app/                        # 应用目录
│   ├── admin/                  # 后台模块
│   │   ├── controller/         # 控制器目录
│   │   │   ├── Auth.php       # 认证控制器
│   │   │   ├── Index.php      # 首页控制器
│   │   │   ├── User.php       # 用户管理控制器
│   │   │   ├── Expert.php     # 大神管理控制器
│   │   │   ├── Category.php   # 分类管理控制器
│   │   │   ├── Product.php    # 商品管理控制器
│   │   │   ├── Order.php      # 订单管理控制器
│   │   │   ├── Comment.php    # 评价管理控制器
│   │   │   ├── Finance.php    # 财务管理控制器
│   │   │   ├── Config.php     # 配置管理控制器
│   │   │   └── system/        # 系统管理子目录
│   │   │       ├── Admin.php   # 管理员管理
│   │   │       ├── Role.php    # 角色管理
│   │   │       ├── Menu.php    # 菜单管理
│   │   │       └── Log.php     # 日志管理
│   │   ├── model/             # 模型目录
│   │   │   ├── User.php       # 用户模型
│   │   │   ├── Expert.php     # 大神模型
│   │   │   ├── Category.php   # 分类模型
│   │   │   ├── Product.php    # 商品模型
│   │   │   ├── Order.php      # 订单模型
│   │   │   └── Config.php     # 配置模型
│   │   ├── service/           # 服务层目录
│   │   │   ├── UserService.php
│   │   │   ├── OrderService.php
│   │   │   └── FinanceService.php
│   │   ├── validate/          # 验证器目录
│   │   │   ├── UserValidate.php
│   │   │   ├── ProductValidate.php
│   │   │   └── OrderValidate.php
│   │   └── view/              # 视图目录
│   │       ├── index/         # 首页视图
│   │       ├── user/          # 用户管理视图
│   │       ├── expert/        # 大神管理视图
│   │       ├── category/      # 分类管理视图
│   │       ├── product/       # 商品管理视图
│   │       ├── order/         # 订单管理视图
│   │       ├── finance/       # 财务管理视图
│   │       └── system/        # 系统管理视图
│   ├── common.php             # 公共函数文件
│   └── event.php              # 事件定义文件
├── config/                     # 配置文件目录
│   ├── app.php                # 应用配置
│   ├── cache.php              # 缓存配置
│   ├── console.php            # 控制台配置
│   ├── cookie.php             # Cookie配置
│   ├── database.php           # 数据库配置
│   ├── middleware.php         # 中间件配置
│   ├── public.php             # 公共配置
│   ├── route.php              # 路由配置
│   ├── session.php            # Session配置
│   ├── trace.php              # 调试配置
│   ├── jwt.php                # JWT配置
│   ├── easyadmin.php          # EasyAdmin核心配置
│   └── addon.php              # 插件配置
├── route/                      # 路由定义目录
│   └── admin.php              # 后台路由
├── public/                     # 网站根目录
│   ├── index.php              # 入口文件
│   ├── admin.php              # 后台入口
│   ├── .htaccess              # Apache伪静态
│   ├── static/                # 静态资源
│   │   ├── admin/             # 后台静态资源
│   │   │   ├── css/           # 样式文件
│   │   │   ├── js/            # 脚本文件
│   │   │   └── images/        # 图片资源
│   │   └── uploads/           # 上传文件
│   └── vendor/                # Composer依赖
├── runtime/                    # 运行时目录
├── vendor/                     # Composer依赖包
├── think                      # 命令行入口
├── composer.json              # Composer配置
└── install.lock              # 安装锁定文件
```

### 1.2 控制器目录结构

```
controller/
├── Auth.php                   # 认证控制器（登录、登出）
├── Index.php                  # 首页控制器（控制台）
├── User.php                   # 用户管理控制器
├── Expert.php                 # 大神管理控制器
├── Category.php               # 游戏分类管理
├── Product.php                # 商品管理控制器
├── Order.php                  # 订单管理控制器
├── Comment.php                # 评价管理控制器
├── Finance.php                # 财务管理控制器
├── Config.php                 # 系统配置控制器
├── Message.php                # 消息管理控制器
├── Statistics.php             # 统计分析控制器
├── Marketing.php              # 营销管理控制器（预留）
├── Report.php                 # 报表管理控制器（预留）
└── system/                    # 系统管理模块
    ├── Admin.php              # 管理员管理
    ├── Role.php               # 角色权限管理
    ├── Menu.php               # 菜单管理
    ├── Config.php             # 系统配置
    ├── Log.php                # 操作日志
    └── Cache.php              # 缓存管理
```

---

## 二、代码编写规范

### 2.1 控制器开发规范

#### 2.1.1 控制器命名规范

| 规范 | 说明 | 示例 |
|------|------|------|
| 文件命名 | 大驼峰命名法 | User.php、OrderDetail.php |
| 类命名 | 大驼峰命名法 | User、OrderDetail |
| 方法命名 | 小驼峰命名法 | index、getList、doSubmit |
| 权限标识 | 小写字母+下划线 | user/index、user/edit |

#### 2.1.2 控制器基础结构

```php
<?php

declare(strict_types=1);

namespace app\admin\controller;

use app\common\controller\BaseAdminController;
use app\service\UserService;
use think\response\Json;

/**
 * 用户管理控制器
 */
class User extends BaseAdminController
{
    /**
     * 当前模型实例
     * @var UserService
     */
    protected UserService $service;

    /**
     * 初始化
     */
    protected function initialize(): void
    {
        parent::initialize();
        $this->service = new UserService();
    }

    /**
     * 用户列表
     */
    public function index(): Json
    {
        if ($this->request->isAjax()) {
            [$where, $page, $limit, $sort] = $this->buildTableParam();
            
            $list = $this->service->getList($where, $page, $limit, $sort);
            $total = $this->service->getCount($where);
            
            return $this->success('获取成功', [
                'list' => $list,
                'total' => $total,
            ]);
        }
        
        return $this->fetch();
    }

    /**
     * 添加用户
     */
    public function add(): Json
    {
        if ($this->request->isPost()) {
            $data = $this->request->post();
            
            // 数据验证
            $result = $this->service->create($data);
            if ($result === false) {
                return $this->error($this->service->getError());
            }
            
            return $this->success('添加成功', ['id' => $result]);
        }
        
        return $this->fetch();
    }

    /**
     * 编辑用户
     */
    public function edit(): Json
    {
        $id = $this->request->param('id/d', 0);
        
        if ($this->request->isPost()) {
            $data = $this->request->post();
            
            $result = $this->service->update($id, $data);
            if ($result === false) {
                return $this->error($this->service->getError());
            }
            
            return $this->success('修改成功');
        }
        
        $data = $this->service->findById($id);
        if (!$data) {
            return $this->error('用户不存在');
        }
        
        $this->assign('data', $data);
        return $this->fetch();
    }

    /**
     * 删除用户
     */
    public function delete(): Json
    {
        $ids = $this->request->param('ids/a', []);
        
        if (empty($ids)) {
            return $this->error('请选择要删除的数据');
        }
        
        $result = $this->service->delete($ids);
        if ($result === false) {
            return $this->error($this->service->getError());
        }
        
        return $this->success('删除成功');
    }

    /**
     * 修改用户状态
     */
    public function status(): Json
    {
        $id = $this->request->param('id/d', 0);
        $status = $this->request->param('status/d', 0);
        
        $result = $this->service->updateStatus($id, $status);
        if ($result === false) {
            return $this->error($this->service->getError());
        }
        
        return $this->success('修改成功');
    }

    /**
     * 查看用户详情
     */
    public function detail(): Json
    {
        $id = $this->request->param('id/d', 0);
        
        $data = $this->service->getDetail($id);
        if (!$data) {
            return $this->error('用户不存在');
        }
        
        return $this->success('获取成功', $data);
    }

    /**
     * 获取字段
     */
    protected function getTableFields(): array
    {
        return [
            ['name' => 'id', 'type' => 'checkbox', 'width' => 50],
            ['name' => 'id', 'type' => 'text', 'title' => 'ID', 'width' => 80],
            ['name' => 'nickname', 'type' => 'text', 'title' => '昵称', 'width' => 120],
            ['name' => 'phone', 'type' => 'text', 'title' => '手机号', 'width' => 120],
            ['name' => 'role_text', 'type' => 'text', 'title' => '角色', 'width' => 80],
            ['name' => 'balance', 'type' => 'text', 'title' => '余额', 'width' => 100],
            ['name' => 'status_text', 'type' => 'tag', 'title' => '状态', 'width' => 80],
            ['name' => 'created_at', 'type' => 'text', 'title' => '注册时间', 'width' => 160],
            ['name' => 'action', 'type' => 'toolbar', 'title' => '操作', 'width' => 150, 'buttons' => [
                ['name' => 'edit', 'title' => '编辑', 'icon' => 'fa fa-edit'],
                ['name' => 'delete', 'title' => '删除', 'icon' => 'fa fa-trash', 'type' => 'confirm'],
            ]],
        ];
    }
}
```

#### 2.1.3 控制器返回格式

| 返回方法 | 说明 | 使用场景 |
|----------|------|----------|
| $this->success() | 成功返回 | 操作成功 |
| $this->error() | 错误返回 | 操作失败 |
| $this->redirect() | 重定向 | 页面跳转 |
| $this->fetch() | 渲染视图 | 展示页面 |

### 2.2 模型开发规范

#### 2.2.1 模型基础结构

```php
<?php

declare(strict_types=1);

namespace app\admin\model;

use app\common\model\BaseModel;
use think\model\relation\HasMany;
use think\model\relation\BelongsTo;

/**
 * 用户模型
 */
class User extends BaseModel
{
    // 表名
    protected $name = 'users';
    
    // 自动时间戳
    protected $autoWriteTimestamp = true;
    protected $createTime = 'created_at';
    protected $updateTime = 'updated_at';
    
    // 类型转换
    protected $type = [
        'balance' => 'decimal:2',
        'role' => 'integer',
        'status' => 'integer',
        'created_at' => 'datetime',
        'updated_at' => 'datetime',
    ];

    // 获取器：角色名称
    public function getRoleTextAttr($value, $data): string
    {
        $roleList = [1 => '用户', 2 => '大神', 3 => '客服', 4 => '管理员'];
        return $roleList[$data['role']] ?? '未知';
    }

    // 获取器：状态名称
    public function getStatusTextAttr($value, $data): string
    {
        $statusList = [0 => '禁用', 1 => '正常'];
        return $statusList[$data['status']] ?? '未知';
    }

    // 获取器：状态标签样式
    public function getStatusTagAttr($value, $data): array
    {
        $style = [
            0 => ['text' => '禁用', 'bg' => 'bg-red'],
            1 => ['text' => '正常', 'bg' => 'bg-green'],
        ];
        return $style[$data['status']] ?? ['text' => '未知', 'bg' => 'bg-gray'];
    }

    // 获取器：头像完整路径
    public function getAvatarFullAttr($value, $data): string
    {
        if (empty($data['avatar'])) {
            return static::DEFAULT_AVATAR;
        }
        return str_starts_with($data['avatar'], 'http') 
            ? $data['avatar'] 
            : static::FILE_DOMAIN . $data['avatar'];
    }

    // 关联订单
    public function orders(): HasMany
    {
        return $this->hasMany(Order::class, 'user_id', 'id');
    }

    // 关联商品
    public function products(): HasMany
    {
        return $this->hasMany(Product::class, 'expert_id', 'id');
    }

    // 关联角色
    public function role(): BelongsTo
    {
        return $this->belongsTo(Role::class, 'role_id', 'id');
    }

    // 搜索器：手机号搜索
    public function searchPhoneAttr(array $query): void
    {
        if (isset($query['phone']) && !empty($query['phone'])) {
            $query['where'][] = ['phone', 'like', '%' . $query['phone'] . '%'];
        }
    }

    // 搜索器：角色筛选
    public function searchRoleAttr(array $query): void
    {
        if (isset($query['role']) && $query['role'] !== '') {
            $query['where'][] = ['role', '=', (int)$query['role']];
        }
    }

    // 搜索器：状态筛选
    public function searchStatusAttr(array $query): void
    {
        if (isset($query['status']) && $query['status'] !== '') {
            $query['where'][] = ['status', '=', (int)$query['status']];
        }
    }

    // 搜索器：时间范围搜索
    public function searchDateRangeAttr(array $query): void
    {
        if (isset($query['start_date']) && !empty($query['start_date'])) {
            $query['where'][] = ['created_at', '>=', $query['start_date'] . ' 00:00:00'];
        }
        if (isset($query['end_date']) && !empty($query['end_date'])) {
            $query['where'][] = ['created_at', '<=', $query['end_date'] . ' 23:59:59'];
        }
    }
}
```

#### 2.2.2 模型常量定义

```php
<?php

namespace app\admin\model;

use app\common\model\BaseModel;

class User extends BaseModel
{
    // 默认头像
    const DEFAULT_AVATAR = 'https://cdn.escortgame.com/default/avatar.png';
    
    // 文件域名
    const FILE_DOMAIN = 'https://cdn.escortgame.com';
    
    // 角色常量
    const ROLE_USER = 1;      // 普通用户
    const ROLE_EXPERT = 2;    // 大神
    const ROLE_SERVICE = 3;   // 客服
    const ROLE_ADMIN = 4;     // 管理员
    
    // 角色映射
    const ROLE_MAP = [
        self::ROLE_USER => '用户',
        self::ROLE_EXPERT => '大神',
        self::ROLE_SERVICE => '客服',
        self::ROLE_ADMIN => '管理员',
    ];
    
    // 状态常量
    const STATUS_DISABLE = 0;  // 禁用
    const STATUS_ENABLE = 1;    // 正常
    
    // 状态映射
    const STATUS_MAP = [
        self::STATUS_DISABLE => '禁用',
        self::STATUS_ENABLE => '正常',
    ];
    
    // 大神认证状态
    const EXPERT_STATUS_NONE = 0;      // 未认证
    const EXPERT_STATUS_PENDING = 1;   // 审核中
    const EXPERT_STATUS_PASS = 2;      // 已认证
    const EXPERT_STATUS_REJECT = 3;    // 已拒绝
    
    // 认证状态映射
    const EXPERT_STATUS_MAP = [
        self::EXPERT_STATUS_NONE => '未认证',
        self::EXPERT_STATUS_PENDING => '审核中',
        self::EXPERT_STATUS_PASS => '已认证',
        self::EXPERT_STATUS_REJECT => '已拒绝',
    ];
}
```

### 2.3 服务层开发规范

#### 2.3.1 服务层基础结构

```php
<?php

declare(strict_types=1);

namespace app\service;

use app\model\User as UserModel;
use app\repository\UserRepository;
use think\exception\ValidateException;

/**
 * 用户服务层
 */
class UserService
{
    protected UserRepository $repository;
    
    public function __construct()
    {
        $this->repository = new UserRepository();
    }

    /**
     * 获取用户列表
     */
    public function getList(array $where = [], int $page = 1, int $limit = 15, string $sort = 'id desc'): array
    {
        $list = $this->repository->getList($where, $page, $limit, $sort);
        
        // 处理数据格式
        foreach ($list as &$item) {
            $item['role_text'] = UserModel::ROLE_MAP[$item['role']] ?? '未知';
            $item['status_text'] = UserModel::STATUS_MAP[$item['status']] ?? '未知';
            $item['status_tag'] = [
                'text' => $item['status_text'],
                'bg' => $item['status'] === 1 ? 'bg-green' : 'bg-red',
            ];
        }
        
        return $list;
    }

    /**
     * 获取用户数量
     */
    public function getCount(array $where = []): int
    {
        return $this->repository->getCount($where);
    }

    /**
     * 创建用户
     */
    public function create(array $data): int|false
    {
        // 验证手机号唯一性
        if ($this->repository->existsByPhone($data['phone'])) {
            throw new ValidateException('手机号已存在');
        }
        
        // 密码加密
        if (isset($data['password'])) {
            $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
        }
        
        return $this->repository->create($data);
    }

    /**
     * 更新用户
     */
    public function update(int $id, array $data): bool
    {
        $user = $this->repository->findById($id);
        if (!$user) {
            throw new ValidateException('用户不存在');
        }
        
        // 如果修改手机号，验证唯一性
        if (isset($data['phone']) && $data['phone'] !== $user['phone']) {
            if ($this->repository->existsByPhone($data['phone'], $id)) {
                throw new ValidateException('手机号已存在');
            }
        }
        
        // 如果修改密码
        if (isset($data['password']) && !empty($data['password'])) {
            $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
        } else {
            unset($data['password']);
        }
        
        return $this->repository->update($id, $data);
    }

    /**
     * 删除用户
     */
    public function delete(array $ids): bool
    {
        // 不能删除自己
        $adminId = session('admin_id');
        if (in_array($adminId, $ids)) {
            throw new ValidateException('不能删除当前登录账号');
        }
        
        return $this->repository->delete($ids);
    }

    /**
     * 修改用户状态
     */
    public function updateStatus(int $id, int $status): bool
    {
        $user = $this->repository->findById($id);
        if (!$user) {
            throw new ValidateException('用户不存在');
        }
        
        return $this->repository->update($id, ['status' => $status]);
    }

    /**
     * 获取用户详情
     */
    public function getDetail(int $id): array
    {
        $user = $this->repository->findById($id);
        if (!$user) {
            throw new ValidateException('用户不存在');
        }
        
        // 关联数据处理
        $user['role_text'] = UserModel::ROLE_MAP[$user['role']] ?? '未知';
        $user['status_text'] = UserModel::STATUS_MAP[$user['status']] ?? '未知';
        $user['expert_status_text'] = UserModel::EXPERT_STATUS_MAP[$user['expert_status']] ?? '未知';
        
        return $user;
    }
}
```

### 2.4 验证器开发规范

#### 2.4.1 验证器基础结构

```php
<?php

declare(strict_types=1);

namespace app\admin\validate;

use think\Validate;

/**
 * 用户验证器
 */
class UserValidate extends Validate
{
    // 验证规则
    protected $rule = [
        'id' => 'require|number|gt:0',
        'phone' => 'require|mobile|length:11',
        'password' => 'require|length:6,20',
        'nickname' => 'require|length:2,20',
        'role' => 'require|in:1,2,3,4',
        'status' => 'require|in:0,1',
        'real_name' => 'length:2,20',
        'id_card' => 'idCard',
    ];

    // 错误消息
    protected $message = [
        'id.require' => 'ID不能为空',
        'id.number' => 'ID必须是数字',
        'phone.require' => '手机号不能为空',
        'phone.mobile' => '手机号格式不正确',
        'password.require' => '密码不能为空',
        'password.length' => '密码长度在6-20位之间',
        'nickname.require' => '昵称不能为空',
        'nickname.length' => '昵称长度在2-20位之间',
        'role.require' => '角色不能为空',
        'role.in' => '角色值不正确',
        'status.require' => '状态不能为空',
        'status.in' => '状态值不正确',
    ];

    // 验证场景
    protected $scene = [
        'add' => ['phone', 'password', 'nickname', 'role'],
        'edit' => ['id', 'phone', 'nickname', 'role', 'status'],
        'status' => ['id', 'status'],
        'delete' => ['id'],
    ];

    // 自定义验证规则：身份证号
    protected function idCard($value): bool
    {
        $pattern = '/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/';
        return preg_match($pattern, $value) === 1;
    }
}
```

### 2.5 视图开发规范

#### 2.5.1 视图基础结构

```php
{extend name="../../common/view/admin/page/index" /}

{block name="css"}
<style>
    .user-avatar {
        width: 40px;
        height: 40px;
        border-radius: 50%;
    }
    .balance-text {
        color: #ef4444;
        font-weight: 600;
    }
</style>
{/block}

{block name="content"}
<div class="layui-fluid">
    <div class="layui-card">
        <!-- 搜索表单 -->
        <div class="layui-card-header">
            <form class="layui-form" lay-filter="search-form">
                <div class="layui-form-item">
                    <div class="layui-inline">
                        <label class="layui-form-label">关键词：</label>
                        <div class="layui-input-inline">
                            <input type="text" name="keyword" placeholder="昵称/手机号" class="layui-input">
                        </div>
                    </div>
                    <div class="layui-inline">
                        <label class="layui-form-label">角色：</label>
                        <div class="layui-input-inline">
                            <select name="role">
                                <option value="">全部</option>
                                <option value="1">用户</option>
                                <option value="2">大神</option>
                                <option value="3">客服</option>
                                <option value="4">管理员</option>
                            </select>
                        </div>
                    </div>
                    <div class="layui-inline">
                        <button type="submit" class="layui-btn layui-btn-normal" lay-submit lay-filter="search">
                            <i class="layui-icon layui-icon-search"></i> 搜索
                        </button>
                        <button type="reset" class="layui-btn layui-btn-primary">重置</button>
                    </div>
                </div>
            </form>
        </div>
        
        <!-- 数据表格 -->
        <div class="layui-card-body">
            <table id="data-table" lay-filter="data-table"></table>
        </div>
    </div>
</div>

<!-- 表格工具栏 -->
<script type="text/html" id="toolbar">
    <div class="layui-btn-container">
        <button class="layui-btn layui-btn-sm layui-btn-normal" lay-event="add">
            <i class="layui-icon layui-icon-add-1"></i> 添加用户
        </button>
    </div>
</script>

<!-- 行工具栏 -->
<script type="text/html" id="action-bar">
    <a class="layui-btn layui-btn-xs" lay-event="edit">编辑</a>
    <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
</script>

<!-- 状态模板 -->
<script type="text/html" id="status-tpl">
    {% if d.status == 1 %}
    <span class="layui-badge layui-bg-green">正常</span>
    {% else %}
    <span class="layui-badge layui-bg-red">禁用</span>
    {% endif %}
</script>

<!-- 头像模板 -->
<script type="text/html" id="avatar-tpl">
    <img src="{{$n.helper.avatar(d.avatar)}}" class="user-avatar" alt="">
</script>

{/block}

{block name="js"}
<script>
    // 初始化表格
    layui.use(['table', 'form', 'layer'], function() {
        var table = layui.table;
        var form = layui.form;
        var layer = layui.layer;
        
        // 渲染表格
        var dataTable = table.render({
            elem: '#data-table',
            url: '{:url("index")}',
            page: true,
            toolbar: '#toolbar',
            defaultToolbar: ['filter', 'exports', 'print'],
            cols: [[
                {type: 'checkbox', fixed: 'left'},
                {field: 'id', title: 'ID', width: 80, sort: true},
                {field: 'avatar', title: '头像', width: 80, templet: '#avatar-tpl'},
                {field: 'nickname', title: '昵称', width: 120},
                {field: 'phone', title: '手机号', width: 120},
                {field: 'role_text', title: '角色', width: 80},
                {field: 'balance', title: '余额', width: 100, templet: function(d) {
                    return '<span class="balance-text">¥' + d.balance + '</span>';
                }},
                {field: 'status', title: '状态', width: 80, templet: '#status-tpl'},
                {field: 'created_at', title: '注册时间', width: 160},
                {fixed: 'right', title: '操作', width: 150, toolbar: '#action-bar'}
            ]]
        });
        
        // 搜索
        form.on('submit(search)', function(data) {
            dataTable.reload({
                where: data.field,
                page: {curr: 1}
            });
            return false;
        });
        
        // 工具栏事件
        table.on('toolbar(data-table)', function(obj) {
            switch(obj.event) {
                case 'add':
                    openForm('{:url("add")}', '添加用户');
                    break;
            }
        });
        
        // 行工具栏事件
        table.on('tool(data-table)', function(obj) {
            var data = obj.data;
            switch(obj.event) {
                case 'edit':
                    openForm('{:url("edit")}?id=' + data.id, '编辑用户');
                    break;
                case 'del':
                    doDelete(data.id);
                    break;
            }
        });
        
        // 打开表单弹窗
        function openForm(url, title) {
            layer.open({
                type: 2,
                title: title,
                area: ['600px', '500px'],
                content: url,
                end: function() {
                    dataTable.reload();
                }
            });
        }
        
        // 删除操作
        function doDelete(ids) {
            layer.confirm('确定要删除选中的用户吗？', function(index) {
                $.post('{:url("delete")}', {ids: [ids]}, function(res) {
                    if (res.code === 200) {
                        layer.msg('删除成功');
                        dataTable.reload();
                    } else {
                        layer.msg(res.msg);
                    }
                });
                layer.close(index);
            });
        }
    });
</script>
{/block}
```

---

## 三、数据表格开发规范

### 3.1 表格配置

#### 3.1.1 基础配置项

| 配置项 | 类型 | 说明 |
|--------|------|------|
| elem | string | 表格容器选择器 |
| url | string | 数据接口地址 |
| cols | array | 表头配置 |
| page | bool/array | 是否分页 |
| limit | int | 每页条数 |
| limits | array | 每页条数选择 |
| toolbar | string | 工具栏模板ID |
| defaultToolbar | array | 默认工具栏 |
| autoSort | bool | 是否自动排序 |
| loading | bool | 是否显示加载动画 |
| skin | string | 表格风格 |
| size | string | 表格尺寸 |
| even | bool | 隔行背景 |

#### 3.1.2 常用配置示例

```javascript
// 基础配置
table.render({
    elem: '#data-table',
    url: '/admin/user/index',
    page: true,
    limit: 15,
    limits: [15, 30, 50, 100],
    toolbar: '#toolbar',
    defaultToolbar: ['filter', 'exports', 'print'],
    autoSort: true,
    loading: true,
    skin: 'line',
    size: 'md',
    even: true,
    cols: [[
        {type: 'checkbox', fixed: 'left'},
        {field: 'id', title: 'ID', width: 80, sort: true, fixed: 'left'},
        {field: 'nickname', title: '昵称', width: 150},
        {field: 'phone', title: '手机号', width: 130},
        {field: 'role_text', title: '角色', width: 100},
        {field: 'balance', title: '余额', width: 120},
        {field: 'status', title: '状态', width: 100, templet: '#statusTpl'},
        {field: 'created_at', title: '创建时间', width: 180, sort: true},
        {fixed: 'right', title: '操作', width: 200, toolbar: '#actionBar', fixed: 'right'}
    ]]
});
```

### 3.2 字段类型

| 类型 | 说明 | 配置项 |
|------|------|--------|
| checkbox | 复选框 | type: 'checkbox' |
| radio | 单选框 | type: 'radio' |
| numbers | 序号列 | type: 'numbers' |
| space | 空列 | type: 'space' |
| text | 文本 | type: 'text' |
| edit | 编辑框 | type: 'edit' |
| select | 下拉框 | type: 'select' |
| switch | 开关 | type: 'switch' |
| date | 日期 | type: 'date' |
| datetime | 日期时间 | type: 'datetime' |
| image | 图片 | type: 'image' |
| images | 图片组 | type: 'images' |
| file | 文件 | type: 'file' |
| templet | 模板渲染 | templet: '#tplId' |
| toolbar | 工具栏 | toolbar: '#toolbarId' |
| total | 统计行 | type: 'numbers', totalRow: true |

### 3.3 分页配置

| 配置项 | 类型 | 说明 |
|--------|------|------|
| page | bool/array | 是否分页 |
| limit | int | 默认每页条数 |
| limits | array | 每页条数选项 |
| prev | string | 上一页文本 |
| next | string | 下一页文本 |
| first | string | 首页文本 |
| last | string | 尾页文本 |
| layout | array | 分页组件布局 |
| count | int | 数据总数 |
| curr | int | 当前页码 |
| groups | int | 连续页码数 |

---

## 四、权限管理规范

### 4.1 权限标识定义

#### 4.1.1 模块权限标识

| 模块 | 权限前缀 | 说明 |
|------|----------|------|
| 首页 | index/ | 控制台 |
| 用户管理 | user/ | 用户相关 |
| 大神管理 | expert/ | 大神相关 |
| 分类管理 | category/ | 游戏分类 |
| 商品管理 | product/ | 商品相关 |
| 订单管理 | order/ | 订单相关 |
| 评价管理 | comment/ | 评价相关 |
| 财务管理 | finance/ | 财务相关 |
| 配置管理 | config/ | 系统配置 |
| 系统管理 | system/ | 系统相关 |

#### 4.1.2 操作权限标识

| 操作 | 权限后缀 | 说明 |
|------|----------|------|
| 列表 | /index | 查看列表 |
| 添加 | /add | 新增数据 |
| 编辑 | /edit | 修改数据 |
| 删除 | /delete | 删除数据 |
| 状态 | /status | 修改状态 |
| 审核 | /review | 审核操作 |
| 导出 | /export | 导出数据 |
| 详情 | /detail | 查看详情 |

#### 4.1.3 完整权限标识示例

```php
<?php

// 权限标识定义
return [
    // 用户管理
    'user/index' => '用户列表',
    'user/add' => '添加用户',
    'user/edit' => '编辑用户',
    'user/delete' => '删除用户',
    'user/status' => '修改用户状态',
    'user/detail' => '查看用户详情',
    'user/export' => '导出用户',
    
    // 大神管理
    'expert/index' => '大神列表',
    'expert/add' => '添加大神',
    'expert/edit' => '编辑大神',
    'expert/delete' => '删除大神',
    'expert/review' => '大神认证审核',
    'expert/status' => '修改大神状态',
    'expert/export' => '导出大神',
    
    // 订单管理
    'order/index' => '订单列表',
    'order/detail' => '订单详情',
    'order/dispatch' => '订单派单',
    'order/finish' => '强制结算',
    'order/refund' => '处理退款',
    'order/export' => '导出订单',
    
    // 财务管理
    'finance/index' => '财务概览',
    'finance/recharge' => '充值记录',
    'finance/withdraw' => '提现管理',
    'finance/flow' => '资金流水',
    'finance/export' => '导出财务',
];
```

### 4.2 角色配置

| 角色名称 | 角色标识 | 权限范围 |
|----------|----------|----------|
| 超级管理员 | super_admin | 全部权限 |
| 运营主管 | operation_manager | 用户、订单、营销、统计 |
| 客服主管 | service_manager | 用户、订单、评价、消息 |
| 财务主管 | finance_manager | 财务、统计 |
| 普通管理员 | admin | 根据岗位分配 |

### 4.3 权限验证

#### 4.3.1 控制器权限注解

```php
<?php

namespace app\admin\controller;

use app\common\annotation\Permission;
use app\common\annotation\Log;

/**
 * 用户管理控制器
 * @Permission("user/index,user/add,user/edit,user/delete")
 * @Log("用户管理")
 */
class User extends BaseAdminController
{
    /**
     * 用户列表
     * @Permission("user/index")
     * @Log("查看用户列表")
     */
    public function index(): Json
    {
        // ...
    }

    /**
     * 添加用户
     * @Permission("user/add")
     * @Log("添加用户")
     */
    public function add(): Json
    {
        // ...
    }

    /**
     * 编辑用户
     * @Permission("user/edit")
     * @Log("编辑用户")
     */
    public function edit(): Json
    {
        // ...
    }

    /**
     * 删除用户
     * @Permission("user/delete")
     * @Log("删除用户")
     */
    public function delete(): Json
    {
        // ...
    }
}
```

---

## 五、扩展开发规范

### 5.1 自定义服务开发

#### 5.1.1 服务层目录结构

```
app/service/
├── UserService.php           # 用户服务
├── ExpertService.php         # 大神服务
├── ProductService.php        # 商品服务
├── OrderService.php          # 订单服务
├── FinanceService.php        # 财务服务
├── MessageService.php        # 消息服务
└── SmsService.php           # 短信服务（自定义）
```

#### 5.1.2 服务注入示例

```php
<?php

namespace app\admin\controller;

use app\common\controller\BaseAdminController;
use app\service\OrderService;
use app\service\FinanceService;
use think\response\Json;

/**
 * 订单管理控制器
 */
class Order extends BaseAdminController
{
    // 服务注入
    protected OrderService $orderService;
    protected FinanceService $financeService;
    
    // 初始化
    protected function initialize(): void
    {
        parent::initialize();
        $this->orderService = app(OrderService::class);
        $this->financeService = app(FinanceService::class);
    }
    
    /**
     * 订单列表
     */
    public function index(): Json
    {
        $list = $this->orderService->getList($params);
        return $this->success('获取成功', $list);
    }
    
    /**
     * 处理退款
     */
    public function refund(): Json
    {
        $orderId = $this->request->param('order_id/d', 0);
        
        // 执行业务逻辑
        Db::startTrans();
        try {
            // 订单退款
            $this->orderService->processRefund($orderId);
            // 更新财务
            $this->financeService->recordRefund($orderId);
            
            Db::commit();
            return $this->success('退款成功');
        } catch (\Exception $e) {
            Db::rollback();
            return $this->error($e->getMessage());
        }
    }
}
```

### 5.2 自定义命令开发

#### 5.2.1 命令目录结构

```
app/command/
├── OrderAutoClose.php        # 订单自动关闭
├── OrderAutoComplete.php      # 订单自动完成
├── ExpertLevelCalc.php        # 大神等级计算
├── StatisticsDaily.php        # 每日统计
└── ClearCache.php            # 清理缓存
```

#### 5.2.2 命令开发示例

```php
<?php

declare(strict_types=1);

namespace app\command;

use app\service\OrderService;
use think\console\Command;
use think\console\Input;
use think\console\input\Argument;
use think\console\input\Option;
use think\console\Output;

/**
 * 订单自动关闭命令
 * 运行方式：php think order:auto-close
 */
class OrderAutoClose extends Command
{
    protected OrderService $orderService;
    
    protected function configure(): void
    {
        $this->setName('order:auto-close')
            ->setDescription('自动关闭超时未支付订单')
            ->addArgument('hours', Argument::OPTIONAL, '超时小时数', 30)
            ->addOption('force', 'f', Option::VALUE_NONE, '强制关闭所有超时订单');
    }

    protected function execute(Input $input, Output $output): int
    {
        $this->orderService = app(OrderService::class);
        
        $hours = (int)$input->getArgument('hours');
        $force = $input->getOption('force');
        
        $output->writeln("开始处理超时{$hours}小时未支付订单...");
        
        $count = $this->orderService->autoCloseTimeoutOrders($hours, $force);
        
        $output->writeln("处理完成，共关闭{$count}个订单。");
        
        return 0;
    }
}
```

### 5.3 自定义中间件开发

#### 5.3.1 中间件目录结构

```
app/middleware/
├── Auth.php                  # 认证中间件
├── Permission.php            # 权限中间件
├── OperationLog.php          # 操作日志中间件
├── RequestRate.php           # 请求限流中间件
└── CrossDomain.php           # 跨域中间件
```

#### 5.3.2 中间件开发示例

```php
<?php

declare(strict_types=1);

namespace app\middleware;

use app\service\AccessLogService;
use think\Request;
use think\Response;

/**
 * 操作日志中间件
 */
class OperationLog
{
    protected AccessLogService $logService;
    
    public function __construct()
    {
        $this->logService = app(AccessLogService::class);
    }

    /**
     * 中间件处理
     */
    public function handle(Request $request, \Closure $next): Response
    {
        // 记录请求前状态
        $startTime = microtime(true);
        
        // 处理请求
        $response = $next($request);
        
        // 记录请求后状态
        $endTime = microtime(true);
        $duration = round(($endTime - $startTime) * 1000, 2);
        
        // 异步记录日志
        $this->logService->record([
            'method' => $request->method(),
            'url' => $request->url(true),
            'ip' => $request->ip(),
            'admin_id' => session('admin_id'),
            'admin_name' => session('admin_name'),
            'params' => json_encode($request->param()),
            'response_code' => $response->getCode(),
            'duration' => $duration,
            'user_agent' => $request->header('user-agent'),
            'created_at' => date('Y-m-d H:i:s'),
        ]);
        
        return $response;
    }
}
```

---

## 六、数据库操作规范

### 6.1 事务操作规范

```php
<?php

// 正确的事务使用方式
public function processOrderRefund(int $orderId): bool
{
    Db::startTrans();
    try {
        // 1. 更新订单状态
        $order = OrderModel::find($orderId);
        $order->status = OrderModel::STATUS_REFUNDED;
        $order->refund_time = time();
        $order->save();
        
        // 2. 退还用户余额
        $user = UserModel::find($order->user_id);
        $user->balance = bcadd($user->balance, $order->amount, 2);
        $user->save();
        
        // 3. 记录资金流水
        FlowModel::create([
            'user_id' => $order->user_id,
            'type' => FlowModel::TYPE_REFUND,
            'amount' => $order->amount,
            'balance_before' => bcsub($user->balance, $order->amount, 2),
            'balance_after' => $user->balance,
            'remark' => "订单{$order->order_no}退款",
            'created_at' => time(),
        ]);
        
        // 4. 扣除大神收益（如有）
        if ($order->expert_id) {
            $expert = UserModel::find($order->expert_id);
            $income = bcmul($order->amount, 0.7, 2); // 假设分成70%
            $expert->balance = bcsub($expert->balance, $income, 2);
            $expert->save();
            
            // 记录大神收益流水
            FlowModel::create([
                'user_id' => $order->expert_id,
                'type' => FlowModel::TYPE_DEDUCT,
                'amount' => -$income,
                'remark' => "订单{$order->order_no}退款扣除",
            ]);
        }
        
        Db::commit();
        return true;
    } catch (\Exception $e) {
        Db::rollback();
        throw new \Exception($e->getMessage());
    }
}
```

### 6.2 批量操作规范

```php
<?php

// 批量更新示例
public function batchUpdateStatus(array $ids, int $status): bool
{
    if (empty($ids)) {
        return false;
    }
    
    return UserModel::whereIn('id', $ids)->update(['status' => $status]);
}

// 批量删除示例
public function batchDelete(array $ids): bool
{
    if (empty($ids)) {
        return false;
    }
    
    // 软删除
    return UserModel::whereIn('id', $ids)->update([
        'status' => -1,
        'deleted_at' => time(),
    ]);
    
    // 硬删除（谨慎使用）
    // return UserModel::whereIn('id', $ids)->delete();
}

// 批量插入示例
public function batchInsert(array $data): int
{
    if (empty($data)) {
        return 0;
    }
    
    $model = new UserModel();
    return $model->insertAll($data);
}
```

---

## 七、接口开发规范

### 7.1 RESTful接口规范

| 请求方法 | 接口路径 | 说明 | 返回数据 |
|----------|----------|------|----------|
| GET | /user | 获取用户列表 | {list, total, page} |
| GET | /user/:id | 获取用户详情 | {data} |
| POST | /user | 创建用户 | {id} |
| PUT | /user/:id | 更新用户 | {success} |
| DELETE | /user/:id | 删除用户 | {success} |
| PATCH | /user/:id/status | 修改用户状态 | {success} |

### 7.2 统一响应格式

```php
<?php

// 成功响应
public function success(string $msg = '操作成功', mixed $data = null): Json
{
    return json([
        'code' => 200,
        'msg' => $msg,
        'data' => $data,
        'time' => time(),
    ]);
}

// 错误响应
public function error(string $msg = '操作失败', int $code = 400, mixed $data = null): Json
{
    return json([
        'code' => $code,
        'msg' => $msg,
        'data' => $data,
        'time' => time(),
    ]);
}
```

### 7.3 错误码定义

| 错误码 | 说明 | HTTP状态码 |
|--------|------|------------|
| 200 | 成功 | 200 |
| 400 | 请求参数错误 | 400 |
| 401 | 未授权 | 401 |
| 403 | 权限不足 | 403 |
| 404 | 资源不存在 | 404 |
| 422 | 数据验证失败 | 422 |
| 500 | 服务器内部错误 | 500 |

---

## 八、日志管理规范

### 8.1 日志类型

| 日志类型 | 说明 | 存储位置 |
|----------|------|----------|
| 操作日志 | 用户操作记录 | database/admin_log |
| 错误日志 | 程序错误记录 | runtime/log/error |
| 访问日志 | 接口访问记录 | runtime/log/access |
| SQL日志 | SQL执行记录 | runtime/log/sql |
| 支付日志 | 支付相关记录 | runtime/log/pay |

### 8.2 日志记录示例

```php
<?php

// 记录操作日志
Log::info('用户登录', [
    'admin_id' => $adminId,
    'username' => $username,
    'ip' => $request->ip(),
    'user_agent' => $request->header('user-agent'),
]);

// 记录错误日志
Log::error('订单处理失败', [
    'order_id' => $orderId,
    'error' => $e->getMessage(),
    'trace' => $e->getTraceAsString(),
]);

// 记录支付日志
Log::channel('pay')->info('支付回调', [
    'order_no' => $orderNo,
    'pay_type' => $payType,
    'amount' => $amount,
    'trade_no' => $tradeNo,
]);
```

---

## 九、开发检查清单

### 9.1 代码规范检查

- [ ] 控制器命名符合规范
- [ ] 模型命名符合规范
- [ ] 服务层方法命名规范
- [ ] 注释完整清晰
- [ ] 代码格式统一

### 9.2 功能检查

- [ ] 增删改查功能正常
- [ ] 数据验证有效
- [ ] 权限验证正确
- [ ] 日志记录完整
- [ ] 异常处理完善

### 9.3 安全检查

- [ ] SQL注入防护
- [ ] XSS防护
- [ ] CSRF防护
- [ ] 权限验证严格
- [ ] 敏感数据加密

### 9.4 性能检查

- [ ] 数据库索引合理
- [ ] 查询效率优化
- [ ] 缓存策略正确
- [ ] 避免N+1查询
- [ ] 大数据分页处理

---

## 十、常用开发技巧

### 10.1 模型快捷查询

```php
<?php

// 根据主键获取
$user = UserModel::find(1);

// 根据条件获取一条
$user = UserModel::where('phone', $phone)->find();

// 获取列表
$list = UserModel::where('status', 1)->select();

// 分页查询
$list = UserModel::paginate(15);

// 统计数量
$count = UserModel::where('status', 1)->count();

// 求和
$total = UserModel::where('status', 1)->sum('balance');

// 获取单字段值
$phone = UserModel::where('id', 1)->value('phone');

// 获取某列所有值
$phones = UserModel::where('status', 1)->column('phone');

// 增改删
UserModel::create($data);
UserModel::update($data, ['id' => 1]);
UserModel::destroy([1, 2, 3]);
```

### 10.2 常用查询构造器

```php
<?php

// 多条件查询
$list = UserModel::where(function($query) use ($params) {
    if (!empty($params['keyword'])) {
        $query->whereOr([
            ['nickname', 'like', '%' . $params['keyword'] . '%'],
            ['phone', 'like', '%' . $params['keyword'] . '%'],
        ]);
    }
    if (!empty($params['role'])) {
        $query->where('role', $params['role']);
    }
    if (!empty($params['status'])) {
        $query->where('status', $params['status']);
    }
})->order('id desc')->paginate(15);

// 关联查询
$list = UserModel::with(['orders', 'profile'])
    ->where('role', 2)
    ->select();

// 聚合查询
$stats = UserModel::whereTime('created_at', 'today')
    ->fieldRaw('role, COUNT(*) as count, SUM(balance) as total')
    ->group('role')
    ->select();
```

---

*文档版本：v1.0*  
*创建日期：2026-05-21*  
*最后更新：2026-05-21*
