[TOC]

Author：[歪瑞古德小队](https://www.cnblogs.com/misterchaos/p/12766888.html)

Project：[海岛漂流](https://www.cnblogs.com/misterchaos/p/12815587.html)

集合贴：[团队作业4：项目冲刺集合贴（歪瑞古德小队）](https://www.cnblogs.com/misterchaos/p/12934217.html)

## 一、Daily Scrum Meeting

### 1.1 会议照片

![image-20200528132813653](http://nextcloud.hellochaos.cn/index.php/s/e57AsWacXXmSMxx/preview)

### 1.2 项目进展

| 团队成员 | 昨日完成任务                                                 | 今日计划任务                                                 | 遇到的困难                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------------- |
| 黄钰朝   | `#41`[随机返回一个海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/41) | `#42`[查询星标的海岛列表](https://github.com/gdut-very-good/island-drifting-backend/issues/42) | 上传图片的功能出现问题                             |
| 黄煜淇   | `#45`[上传海岛背景](https://github.com/gdut-very-good/island-drifting-backend/issues/45) | `#48`[海岛动态模块开发](https://github.com/gdut-very-good/island-drifting-backend/issues/48) | redis配置bind时理解错了意思，导致外网无法连接redis |
| 余圣源   | `#52`[整体界面优化](https://github.com/gdut-very-good/island-drifting-backend/issues/52) | `#53`[打包成为安卓应用](https://github.com/gdut-very-good/island-drifting-backend/issues/53) | 不知道怎么解决打包之后，出现的渲染错误             |
| 张文俊   | `#59`[整体页面优化](https://github.com/gdut-very-good/island-drifting-backend/issues/59) | `#60`[协助大佬打包成安卓应用](https://github.com/gdut-very-good/island-drifting-backend/issues/60) | 路由切换时传递参数有困难                           |
| 丘丽珊   | `#66`[绘制第六天站立会议照片 ](https://github.com/gdut-very-good/island-drifting-backend/issues/66) | `#67`[绘制第七天站立会议照片，一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/67) | 难以保证工作同步的时效性                           |
| 陈宇     | `#74`[查询一条动态下面的评论和回复](https://github.com/gdut-very-good/island-drifting-backend/issues/74) | `#75`[在一条海岛动态下发布评论/回复 ](https://github.com/gdut-very-good/island-drifting-backend/issues/75) | 最后一天没有困难！愿天堂没有总结和困难             |

## 二、项目燃尽图

![05-27燃尽图](http://nextcloud.hellochaos.cn/index.php/s/JABd8Zi5BbdHCeY/preview)

## 三、签入记录

### 3.1 代码/文档签入记录

![image-20200526221002219](http://nextcloud.hellochaos.cn/index.php/s/3x7AM7GxN3iMkC8/preview)

### 3.2 Code Review 记录

![image-20200526220902363](http://nextcloud.hellochaos.cn/index.php/s/gNW74WarJ2BGY2D/preview)

![image-20200526181355291](http://nextcloud.hellochaos.cn/index.php/s/yY575jTWfNBEFnJ/preview)

![image-20200526181508043](http://nextcloud.hellochaos.cn/index.php/s/z7ff7GeRZiWiJDp/preview)

![image-20200526181655180](http://nextcloud.hellochaos.cn/index.php/s/NetGdJWptj2WmsG/preview)

### 3.3 issue内容和链接

| 团队成员 | issue内容和链接                                              |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | `#41`[随机返回一个海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/41) |
| 黄煜淇   | `#45`[上传海岛背景](https://github.com/gdut-very-good/island-drifting-backend/issues/45) |
| 余圣源   | `#52`[整体界面优化](https://github.com/gdut-very-good/island-drifting-backend/issues/52) |
| 张文俊   | `#59`[整体页面优化](https://github.com/gdut-very-good/island-drifting-backend/issues/59) |
| 丘丽珊   | `#66`[绘制第六天站立会议照片 ](https://github.com/gdut-very-good/island-drifting-backend/issues/66) |
| 陈宇     | `#74`[查询一条动态下面的评论和回复](https://github.com/gdut-very-good/island-drifting-backend/issues/74) |

## 四、最新模块截图

### 4.1 最新模块代码

（1）上传海岛背景

```java
   @Override
    public String uploadBackground(MultipartFile file, Integer userId) {
        log.info("正在执行上传用户海岛背景操作");

        File result = checkAndUploadFile(file);

        // 进行数据库的更新
        User userPo = getOne(new QueryWrapper<User>().eq("user_id", userId));

        // 删除旧的背景地址
        if (!StringUtils.isEmpty(userPo.getBackground())) {
            UploadUtils.deleteFile(userPo.getBackground());
        }

        // 设置图片地址
        userPo.setBackground(result.getName());

        updateById(userPo);

        return result.getName();
    }
```

（2）查询一条动态下面的评论回复

```java
@Override
    public List<Reply> getByPostId(Integer id) {
        log.info("正在查询reply中postId为{}的数据", id);
        Map<String,Object> map=new HashMap<>();
        map.put("post_id",id);
        List<Reply> reply=super.listByMap(map);
        log.info("postId为{}查询reply完毕: 结果数 = {} ",id,reply.size() );
        return reply;
    }
```

### 4.2 程序运行截图

![image-20200526221127844](http://nextcloud.hellochaos.cn/index.php/s/WXxpeyyc473m7bR/preview)![image-20200526221149404](http://nextcloud.hellochaos.cn/index.php/s/fdtzLgKSGHdoiff/preview)

![image-20200526221216102](http://nextcloud.hellochaos.cn/index.php/s/5RSStaKR9GDRNpc/preview)![image-20200526221311882](http://nextcloud.hellochaos.cn/index.php/s/rNrnJYsWqCnzAj7/preview)

## 五、每日总结

| 团队成员 | 总结内容                                                     |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | 整个项目的计划基本如期执行，比较顺利                         |
| 黄煜淇   | 完成了操作系统的实验，更加了解了redis的配置                  |
| 余圣源   | 以后每一个阶段都进行一次打包测试，否则后面内容多了，排查错误比较困难 |
| 张文俊   | 使用动态路由                                                 |
| 丘丽珊   | 很快就要完工了，继续加油，开心!                              |
| 陈宇     | 不是我的模块也可以帮忙写一写，团队合作互帮互助！             |



