[TOC]

Author：[歪瑞古德小队](https://www.cnblogs.com/misterchaos/p/12766888.html)

Project：[海岛漂流](https://www.cnblogs.com/misterchaos/p/12815587.html)

集合贴：[团队作业4：项目冲刺集合贴（歪瑞古德小队）](https://www.cnblogs.com/misterchaos/p/12934217.html)

## 一、Daily Scrum Meeting

### 1.1 会议照片

![image-20200528132745567](http://nextcloud.hellochaos.cn/index.php/s/RZ5TAJXfsP3zcfq/preview)

### 1.2 项目进展

| 团队成员 | 昨日完成任务                                                 | 今日计划任务                                                 | 遇到的困难                           |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------ |
| 黄钰朝   | `#40`[根据id返回一个海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/40) | `#41`[随机返回一个海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/41) | 操作系统实验和Java<br>作业还没做     |
| 黄煜淇   | `#44`[根据id星标海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/44) | `#45`[上传海岛背景](https://github.com/gdut-very-good/island-drifting-backend/issues/45) | 操作系统实验实现有点难度             |
| 余圣源   | `#51`[完成写信界面和功能接入](https://github.com/gdut-very-good/island-drifting-backend/issues/51) | `#52`[整体界面优化](https://github.com/gdut-very-good/island-drifting-backend/issues/52) | 菜单按钮旋转动画<br>想了比较久才想到 |
| 张文俊   | `#58`[完成发布动态，评论功能的接入](https://github.com/gdut-very-good/island-drifting-backend/issues/58) | `#59`[整体页面优化](https://github.com/gdut-very-good/island-drifting-backend/issues/59) | git合并时代码冲突                    |
| 丘丽珊   | `#65`[绘制第五天站立会议<br/>照片，一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/65) | `#66`[绘制第六天站立会议照片 ](https://github.com/gdut-very-good/island-drifting-backend/issues/66) | 作业多，课程多，<br>时间少           |
| 陈宇     | `#73`[查看一个海岛的动态列表](https://github.com/gdut-very-good/island-drifting-backend/issues/73) | `#74`[查询一条动态下面的评论和回复](https://github.com/gdut-very-good/island-drifting-backend/issues/74) | 操作系统实验真难                     |

## 二、项目燃尽图

![05-26燃尽图](http://nextcloud.hellochaos.cn/index.php/s/piwXewNZZS2ZcbE/preview)

## 三、签入记录

### 3.1 代码/文档签入记录

![image-20200525123735950](http://nextcloud.hellochaos.cn/index.php/s/CdwmBXtBHgMjfib/preview)

### 3.2 Code Review 记录

![image-20200525113505066](http://nextcloud.hellochaos.cn/index.php/s/YQ6yH9cnX9eEdAr/preview)

![image-20200525115533393](http://nextcloud.hellochaos.cn/index.php/s/ZBZAM4CRgkqAYTi/preview)

### 3.3 issue内容和链接

| 团队成员 | issue内容和链接                                              |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | `#40`[根据id返回一个海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/40) |
| 黄煜淇   | `#44`[根据id星标海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/44) |
| 余圣源   | `#51`[完成写信界面和功能接入](https://github.com/gdut-very-good/island-drifting-backend/issues/51) |
| 张文俊   | `#58`[完成发布动态，评论功能的接入](https://github.com/gdut-very-good/island-drifting-backend/issues/58) |
| 丘丽珊   | `#65`[绘制第五天站立会议<br/>照片，一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/65) |
| 陈宇     | `#73`[查看一个海岛的动态列表](https://github.com/gdut-very-good/island-drifting-backend/issues/73) |

## 四、最新模块截图

### 4.1 最新模块代码

（1）星标海岛

```java
    @Override
    public int insertStar(Star star, Integer userId) {
        log.info("正在插入star");

        // 查询海岛id是否为空
        if (star == null || star.getIslandId() == null) {
            log.info("插入star时参数不足");
            throw new BizException("海岛ID不能为空！");
        }

        // 查看海岛是否存在
        if (userMapper.selectCount(new QueryWrapper<User>()
                .eq("user_id", star.getIslandId())) == 0) {
            log.info("星标海岛时id为【{}】的海岛不存在", star.getIslandId());
            throw new BizException("星标的海岛不存在！");
        }

        // 查看是否星标的是自己的海岛
        if (star.getIslandId().equals(userId)) {
            log.info("星标海岛时用户【{}】尝试星标自己的海岛", userId);
            throw new BizException("不可以星标自己的海岛");
        }

        // 查看自己是否已经星标了该海岛
        if (super.count(new QueryWrapper<Star>()
                .eq("user_id", userId)
                .eq("island_id", star.getIslandId())) > 0) {
            log.info("星标海岛时用户【{}】已星标了【{}】海岛", userId, star.getIslandId());
            throw new BizException("您已经星标了该海岛");
        }

        // 进行星标操作
        star.setUserId(userId);

        if (super.save(star)) {
            if (super.save(star)) {
                log.info("插入star成功,id为{}", star.getStarId());
                return star.getStarId();
            }
        }
    }
```

### 4.2 程序运行截图

![image-20200525123809742](http://nextcloud.hellochaos.cn/index.php/s/TWrtzFR5W9mLgid/preview)

## 五、每日总结

| 团队成员 | 总结内容                                                     |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | 作业依然很多...                                              |
| 黄煜淇   | 阅读操作系统书籍，上网了查阅资料辅助实现操作系统             |
| 余圣源   | css动画属性不够熟练，还需要加强使用练习                      |
| 张文俊   | 本地处理后再提交                                             |
| 丘丽珊   | 离终点又进步了一点点，开心!                                  |
| 陈宇     | 操作系统的实验占据了大部分时间，需要分配好时间，兼顾项目和其他课程 |



