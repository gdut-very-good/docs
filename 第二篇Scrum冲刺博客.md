[TOC]

Author：[歪瑞古德小队](https://www.cnblogs.com/misterchaos/p/12766888.html)

Project：[海岛漂流](https://www.cnblogs.com/misterchaos/p/12815587.html)

集合贴：[团队作业4：项目冲刺集合贴（歪瑞古德小队）](https://www.cnblogs.com/misterchaos/p/12934217.html)

## 一、Daily Scrum Meeting

### 1.1 会议照片

![image-20200524172811008](http://nextcloud.hellochaos.cn/index.php/s/5nBGPWrLptKNx9H/preview)

### 1.2 项目进展

| 团队成员 | 昨日完成任务                                                 | 今日计划任务                                                 | 遇到的困难                                                   |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 黄钰朝   | `#33`[统计用户写过的字数](https://github.com/gdut-very-good/island-drifting-backend/issues/33) | `#34`[统计用户发送的信件<br>数量，接收的信件数量](https://github.com/gdut-very-good/island-drifting-backend/issues/34) | 用excel画项目的燃尽图时纵坐标<br>没有值的点也被显示在折线中  |
| 黄煜淇   | `#35`[redis实现定时任务工具类](https://github.com/gdut-very-good/island-drifting-backend/issues/35) | `#37`[完成自定义任务的编写](https://github.com/gdut-very-good/island-drifting-backend/issues/37) | redis序列化localdatetime时<br>出现错误，在时间的序列化上<br>需要进行处理 |
| 余圣源   | `#46`[完成我的邮票界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/46) | `#47`[完成消息界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/47) | 根据首字符拼音进行笔友排序<br>不容易实现                     |
| 张文俊   | `#54`[完成草稿箱界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/54) | `#55`[完成海岛列表界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/55) | 不同分辨率屏幕的适配                                         |
| 丘丽珊   | `#61`[绘制邮票样式](https://github.com/gdut-very-good/island-drifting-backend/issues/61) | `#62`[绘制第二天站立会议照片](https://github.com/gdut-very-good/island-drifting-backend/issues/62) | 做UI的时候风格统一很<br>困难 要整体兼顾                      |
| 陈宇     | `#68`[用户创建一个树洞](https://github.com/gdut-very-good/island-drifting-backend/issues/68) | `#70`[用户删除已经创建的树洞](https://github.com/gdut-very-good/island-drifting-backend/issues/70) | 创建树洞前要判断用户<br>拥有树洞的数量。                     |

## 二、项目燃尽图

![image-20200522171315232](http://nextcloud.hellochaos.cn/index.php/s/CqpLxky5zrTYxKa/preview)

## 三、签入记录

### 3.1 代码/文档签入记录

![image-20200522120948942](http://nextcloud.hellochaos.cn/index.php/s/bBeRgxafD98JNji/preview)

### 3.2 Code Review 记录

![image-20200522161136650](http://nextcloud.hellochaos.cn/index.php/s/s6846ATpnMqNetJ/preview)

![image-20200522161302408](http://nextcloud.hellochaos.cn/index.php/s/8oJ5nfx2eZMz4Wy/preview)

### 3.3 issue内容和链接

| 团队成员 | issue内容和链接                                              |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | `#33`[统计用户写过的字数](https://github.com/gdut-very-good/island-drifting-backend/issues/33) |
| 黄煜淇   | `#35`[redis实现定时任务工具类](https://github.com/gdut-very-good/island-drifting-backend/issues/35) |
| 余圣源   | `#46`[完成我的邮票界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/46) |
| 张文俊   | `#54`[完成草稿箱界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/54) |
| 丘丽珊   | `#61`[绘制邮票样式](https://github.com/gdut-very-good/island-drifting-backend/issues/61) |
| 陈宇     | `#68`[用户创建一个树洞](https://github.com/gdut-very-good/island-drifting-backend/issues/68) |

## 四、最新模块截图

### 4.1 最新模块代码

**信件模块**

```java
/**
* 创建发信任务
*
* @param letter 要发送的信件
* @name scheduleLetterSending
* @notice none
* @author <a href="mailto:kobe524348@gmail.com">黄钰朝</a>
* @date 2020-05-21
*/
private void scheduleLetterSending(Letter letter) {
        log.info("正在创建发信任务");

        //计算收信时间
        User sender = userMapper.selectById(letter.getSenderId());
        User receiver = userMapper.selectById(letter.getReceiverId());
        if (sender == null || receiver == null) {
            log.error("缺少寄件人或收件人，无法发信");
            throw new BizException("发信失败，缺少寄件人或收件人");
        }
        if (sender.getCity() == null || receiver.getCity() == null) {
            log.error("缺少位置信息，无法发信");
            throw new BizException("发信失败，寄件人或收信人缺少位置信息");
        }
        long distance = locationUtils.getDistance(sender.getCity(), receiver.getCity());
        log.info("计算出两者的距离为：{}米", distance);

        //消耗邮票
        if (letter.getSenderId() != null) {
            Stamp stamp = stampMapper.selectById(letter.getStampId());
            if (stamp == null || !stamp.getUserId().equals(sender.getUserId())) {
                log.warn("id为{}的信件没有使用有效邮票，无法发信", letter.getLetterId());
                throw new BizException("发信失败，缺少有效的邮票");
            }
        } else {
            log.warn("id为{}的信件没有使用邮票", letter.getLetterId());
            throw new BizException("请选择一张邮票进行发信!");
        }

        UpdateWrapper<Stamp> stampUpdateWrapper = new UpdateWrapper<>();
        stampUpdateWrapper.eq("stamp_id", letter.getStampId())
                //设置为null,既不属于发信人也不属于收信人
                .set("user_id", null);
        stampMapper.update(new Stamp(), stampUpdateWrapper);

        //统计书写字数
        if (letter.getContent() == null || letter.getContent().trim().isEmpty()) {
            log.warn("不允许插入空的信件");
            throw new BizException("无法发送空的信件");
        }
        sender.setWord(sender.getWord() + letter.getContent().trim().length());
        userMapper.updateById(sender);

        //启动定时任务
        log.info("正在启动定时任务");
        taskScheduler.schedule(new LetterSendingTask(letter, sender),
                calculateDuration(distance));
}
```

**树洞模块**

```java

/**
 * <p>
 * 树洞
 * 服务类
 * </p>
 *
 * @author chaos
 * @since 2020-05-21
 */
public interface TreeHoleService {

    /**
     * 分页查询TreeHole
     *
     * @param page     当前页数
     * @param pageSize 页的大小
     * @param factor   搜索关键词
     * @return 返回mybatis-plus的Page对象,其中records字段为符合条件的查询结果
     * @author chaos
     * @since 2020-05-21
     */
    Page<TreeHole> listTreeHolesByPage(int page, int pageSize, String factor);

    /**
     * 根据id查询TreeHole
     *
     * @param id 需要查询的TreeHole的id
     * @return 返回对应id的TreeHole对象
     * @author chaos
     * @since 2020-05-21
     */
    TreeHole getTreeHoleById(int id);

    /**
     * 插入TreeHole
     *
     * @param treeHole 需要插入的TreeHole对象
     * @return 返回插入成功之后TreeHole对象的id
     * @author chaos
     * @since 2020-05-21
     */
    int insertTreeHole(TreeHole treeHole);

    /**
     * 根据id删除TreeHole
     *
     * @param id 需要删除的TreeHole对象的id
     * @return 返回被删除的TreeHole对象的id
     * @author chaos
     * @since 2020-05-21
     */
    int deleteTreeHoleById(int id, Integer userId);

    /**
     * 根据id更新TreeHole
     *
     * @param treeHole 需要更新的TreeHole对象
     * @return 返回被更新的TreeHole对象的id
     * @author chaos
     * @since 2020-05-21
     */
    int updateTreeHole(TreeHole treeHole);

}

```

### 4.2 程序运行截图

![image-20200522132710149](http://nextcloud.hellochaos.cn/index.php/s/nycx8CiWgJdm5jK/preview)![image-20200522132948306](http://nextcloud.hellochaos.cn/index.php/s/Xr9kWnG5aftS2Hm/preview)

## 五、每日总结

| 团队成员 | 总结内容                                                     |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | 学习到燃尽图的含义和画法，以及规划了未来六天的任务，收获很多 |
| 黄煜淇   | 学习了redis实现定时任务并简单进行了实现                      |
| 余圣源   | 学习并制作了菜单按钮旋转动画，增强了业务处理能力             |
| 张文俊   | 学习了less函数的各种花式操作                                 |
| 丘丽珊   | 巩固了Ai和Axure的用法，学会了Axure的触发控件的使用           |
| 陈宇     | 运用了curd进行业务处理                                       |



