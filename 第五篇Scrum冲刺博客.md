[TOC]

Author：[歪瑞古德小队](https://www.cnblogs.com/misterchaos/p/12766888.html)

Project：[海岛漂流](https://www.cnblogs.com/misterchaos/p/12815587.html)

集合贴：[团队作业4：项目冲刺集合贴（歪瑞古德小队）](https://www.cnblogs.com/misterchaos/p/12934217.html)

## 一、Daily Scrum Meeting

### 1.1 会议照片

![image-20200528132709819](http://nextcloud.hellochaos.cn/index.php/s/JfjHRjTHqmZPHJy/preview)

### 1.2 项目进展

| 团队成员 | 昨日完成任务                                                 | 今日计划任务                                                 | 遇到的困难                                       |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------ |
| 黄钰朝   | `#38`[根据名称搜索海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/38) | `#40`[根据id返回一个海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/40) | 我不知道要交概率论<br>作业，我太难了             |
| 黄煜淇   | `#43`[查询用户信息时返回距离](https://github.com/gdut-very-good/island-drifting-backend/issues/43) | `#44`[根据id星标海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/44) | 概率论作业有点难做，<br>看着答案都不懂           |
| 余圣源   | `#50`[完成树洞和时间胶囊功能接入](https://github.com/gdut-very-good/island-drifting-backend/issues/50) | `#51`[完成写信界面和功能接入](https://github.com/gdut-very-good/island-drifting-backend/issues/51) | 太多的任务，时间真的<br>不够用，设计的功能太多了 |
| 张文俊   | `#57`[完成帖子界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/57) | `#58`[完成发布动态，评论功能的接入](https://github.com/gdut-very-good/island-drifting-backend/issues/58) | 布局设计                                         |
| 丘丽珊   | `#64`[绘制第四天站立会议照片](https://github.com/gdut-very-good/island-drifting-backend/issues/64) | `#65`[绘制第五天站立会议<br>照片，一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/65) | 作业好多，时间不够用                             |
| 陈宇     | `#72`[用户查看树洞内容](https://github.com/gdut-very-good/island-drifting-backend/issues/72) | `#73`[查看一个海岛的动态列表](https://github.com/gdut-very-good/island-drifting-backend/issues/73) | 需要同时兼顾操作系统实验                         |

## 二、项目燃尽图

![05-25燃尽图](http://nextcloud.hellochaos.cn/index.php/s/oLRGmJ2Xj9itmbe/preview)

## 三、签入记录

### 3.1 代码/文档签入记录

![image-20200524121155725](http://nextcloud.hellochaos.cn/index.php/s/7saZgWg5eLiskxx/preview)

### 3.2 Code Review 记录

![image-20200524103156521](http://nextcloud.hellochaos.cn/index.php/s/S2HWTadQnga9CWd/preview)

![image-20200524120722875](http://nextcloud.hellochaos.cn/index.php/s/j2CNYto3Rm937DP/preview)

### 3.3 issue内容和链接

| 团队成员 | issue内容和链接                                              |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | `#38`[根据名称搜索海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/38) |
| 黄煜淇   | `#43`[查询用户信息时返回距离](https://github.com/gdut-very-good/island-drifting-backend/issues/43) |
| 余圣源   | `#50`[完成树洞和时间胶囊功能接入](https://github.com/gdut-very-good/island-drifting-backend/issues/50) |
| 张文俊   | `#57`[完成帖子界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/57) |
| 丘丽珊   | `#64`[绘制第四天站立会议照片](https://github.com/gdut-very-good/island-drifting-backend/issues/64) |
| 陈宇     | `#72`[用户查看树洞内容](https://github.com/gdut-very-good/island-drifting-backend/issues/72) |

## 四、最新模块截图

### 4.1 最新模块代码

（1）搜索海岛

```java
    /**
     * 分页查询User
     *
     * @param page     当前页数
     * @param pageSize 页的大小
     * @param factor   搜索关键词
     * @return 返回mybatis-plus的Page对象,其中records字段为符合条件的查询结果
     * @author chaos
     * @since 2020-05-04
     */
    @Override
    public Page<UserVo> searchUsersByPage(int page, int pageSize, String factor) {
        log.info("正在执行分页查询user: page = {} pageSize = {} factor = {}", page, pageSize, factor);
        QueryWrapper<User> queryWrapper = new QueryWrapper<User>().like("nickname", factor);
        Page<User> result = super.page(new Page<>(page, pageSize), queryWrapper);
        log.info("分页查询user完毕: 结果数 = {} ", result.getRecords().size());

        // 查看自己的信息
        User self = (User) SecurityUtils.getSubject().getPrincipal();

        //转成vo类型
        List<User> records = result.getRecords();
        List<UserVo> voRecords = new LinkedList<>();
        for (User user : records) {
            UserVo userVo = new UserVo();
            BeanUtils.copyProperties(user, userVo);
            userVo.setDistance(locationUtils.getDistance(self.getCity(), user.getCity()));
            voRecords.add(userVo);
        }

        Page<UserVo> userVoPage = new Page<>();
        BeanUtils.copyProperties(result, userVoPage);
        userVoPage.setRecords(voRecords);
        return userVoPage;
    }

 @Scheduled(cron = "0 15 * * * ?")
    public void cacheUsers() {
        log.info("正在清空缓存的用户数据");
        if (users != null) {
            users.clear();
        }
        log.info("正在缓存用户");
        users = super.list();
        log.info("缓存成功：{}条用户数据", users.size());
    }
```

（2）树洞模块

```java

/**
 * <p>
 * 树洞
 * 服务实现类
 * </p>
 *
 * @author chaos
 * @since 2020-05-21
 */
@Slf4j
@Service
public class TreeHoleServiceImpl extends ServiceImpl<TreeHoleMapper, TreeHole> implements TreeHoleService {

    @Resource
    TreeHoleMapper treeHoleMapper;

    @Resource
    MessageMapper messageMapper;

    @Override
    public Page<TreeHole> listTreeHolesByPage(int page, int pageSize) {
        log.info("正在执行分页查询treeHole: page = {} pageSize = {} ", page, pageSize);
        //TODO 这里需要自定义用于匹配的字段,并把wrapper传入下面的page方法
        Page<TreeHole> result = super.page(new Page<>(page, pageSize));
        log.info("分页查询treeHole完毕: 结果数 = {} ", result.getRecords().size());
        return result;
    }

    @Override
    public TreeHoleVo getTreeHoleById(int id) {
        log.info("正在查询treeHole中id为{}的数据", id);
        TreeHole treeHole = super.getById(id);
        if(treeHole==null){
            return null;
        }
        TreeHoleVo treeHoleVo=new TreeHoleVo();
        treeHoleVo.setHole(treeHole);
        Map<String,Object> map=new HashMap<>();
        map.put("tree_hole_id",id);
        treeHoleVo.setMessages(messageMapper.selectByMap(map));
        log.info("查询id为{}的treeHole{}", id, (null == treeHole ? "无结果" : "成功"));
        return treeHoleVo;
    }

    @Override
    public int insertTreeHole(TreeHole treeHole) {
        log.info("正在插入treeHole");
        treeHole.setCreateTime(LocalDateTime.now());
        Integer userTreeNumber=treeHoleMapper.getUserTreeNumber(treeHole.getCreatorId());
        if(userTreeNumber>=5){
            log.error("插入treeHole失败，用户树洞超过五个");
            throw new BizException("用户树洞已有5个，无法添加");
        }
        if (super.save(treeHole)) {
            log.info("插入treeHole成功,id为{}", treeHole.getTreeHoleId());
            return treeHole.getTreeHoleId();
        } else {
            log.error("插入treeHole失败");
            throw new BizException("添加失败");
        }
    }

    @Override
    public int deleteTreeHoleById(int id,Integer userId) {
        log.info("正在删除id为{}的treeHole", id);
        if(!userId.equals(treeHoleMapper.getUserIdByTreeId(id))){
            log.error("更新id为{}的treeHole失败,登录用户与树洞拥有用户不同",id);
            throw new BizException("登录用户与树洞拥有用户不同");
        }
        if (super.removeById(id)) {
            log.info("删除id为{}的treeHole成功", id);
            return id;
        } else {
            log.error("删除id为{}的treeHole失败", id);
            throw new BizException("删除失败[id=" + id + "]");
        }
    }

    @Override
    public int updateTreeHole(TreeHole treeHole) {
        log.info("正在更新id为{}的treeHole", treeHole.getTreeHoleId());
        Integer userId=treeHoleMapper.getUserIdByTreeId(treeHole.getTreeHoleId());
        if(!userId.equals(treeHole.getCreatorId())){
            log.error("更新id为{}的treeHole失败,登录用户与树洞拥有用户不同", treeHole.getTreeHoleId());
            throw new BizException("登录用户与树洞拥有用户不同");
        }
        if (super.updateById(treeHole)) {
            log.info("更新d为{}的treeHole成功", treeHole.getTreeHoleId());
            return treeHole.getTreeHoleId();
        } else {
            log.error("更新id为{}的treeHole失败", treeHole.getTreeHoleId());
            throw new BizException("更新失败[id=" + treeHole.getTreeHoleId() + "]");
        }
    }

    @Override
    public List<TreeHole> getByUserId(Integer userId) {
        log.info("正在查询用户id为{}的树洞",userId);
        Map<String, Object> map=new HashMap<>();
        map.put("creator_id",userId);
        return treeHoleMapper.selectByMap(map);
    }

}

```

### 4.2 程序运行截图

![image-20200524225907996](http://nextcloud.hellochaos.cn/index.php/s/axGTXp5MygH4s4d/preview)![image-20200524230003829](http://nextcloud.hellochaos.cn/index.php/s/5oFQmSJKddZMam4/preview)

## 五、每日总结

| 团队成员 | 总结内容                                                     |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | 开发进度比较顺利，就是作业太多                               |
| 黄煜淇   | 今天一点代码工作都没有,早上进行了积极分子讨论会议，下午复习了操作系统。 |
| 余圣源   | 完善了树洞的功能，写了下fifo算法，发现与题目要求有偏差，重写，很不开心 |
| 张文俊   | flex天下第一                                                 |
| 丘丽珊   | 离终点又进步了一点点，开心!                                  |
| 陈宇     | 今天早上就完成任务，提高了效率                               |



