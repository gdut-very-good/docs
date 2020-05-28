[TOC]

Author：[歪瑞古德小队](https://www.cnblogs.com/misterchaos/p/12766888.html)

Project：[海岛漂流](https://www.cnblogs.com/misterchaos/p/12815587.html)

集合贴：[团队作业4：项目冲刺集合贴（歪瑞古德小队）](https://www.cnblogs.com/misterchaos/p/12934217.html)

## 一、Daily Scrum Meeting

### 1.1 会议照片

![image-20200528132628518](http://nextcloud.hellochaos.cn/index.php/s/XSXfx26sFSCQ4oj/preview)

### 1.2 项目进展

| 团队成员 | 昨日完成任务                                                 | 今日计划任务                                                 | 遇到的困难                                                   |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 黄钰朝   | `#36`[改进定时发信功能的实现](https://github.com/gdut-very-good/island-drifting-backend/issues/36) | `#38`[根据名称搜索海岛](https://github.com/gdut-very-good/island-drifting-backend/issues/38) | 发信任务使用redis进行存取时，<br>由于其中的Mapper属性没有<br>实现序列化接口导致序列化失败 |
| 黄煜淇   | `#39`[时间胶囊模块的开发](https://github.com/gdut-very-good/island-drifting-backend/issues/39) | `#43`[查询用户信息时返回距离](https://github.com/gdut-very-good/island-drifting-backend/issues/43) | 在时间分配上存在困难，<br>多项事务同时存在，<br>导致很难进行代码工作 |
| 余圣源   | `#49`[完成头像上按钮样式修改](https://github.com/gdut-very-good/island-drifting-backend/issues/49) | `#50`[完成树洞和时间胶囊功能接入](https://github.com/gdut-very-good/island-drifting-backend/issues/50) | 临近期末，所有任务同时进行，<br>每天的任务压哨完成，<br>质量不够高 |
| 张文俊   | `#56`[完成海岛界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/56) | `#57`[完成帖子界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/57) | 作业好多，开发时间较少                                       |
| 丘丽珊   | `#63`[绘制第三天站立会议照片，<br/>一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/63) | `#64`[绘制第四天站立会议照片](https://github.com/gdut-very-good/island-drifting-backend/issues/64) | 难以保证工作同步的<br/>时效性，将努力在周末<br/>时间跟上消息更替速度；<br/>作业好多，时间好少，<br/>课程好多。 |
| 陈宇     | `#71`[用户在树洞下留言](https://github.com/gdut-very-good/island-drifting-backend/issues/71) | `#72`[用户查看树洞内容](https://github.com/gdut-very-good/island-drifting-backend/issues/72) | commit的时候太粗心                                           |

## 二、项目燃尽图

![05-24燃尽图](http://nextcloud.hellochaos.cn/index.php/s/dsS2MoYaFSogaw9/preview)

## 三、签入记录

### 3.1 代码/文档签入记录

![image-20200524000020013](http://nextcloud.hellochaos.cn/index.php/s/QFLPfyaG9dx3Ytw/preview)

### 3.2 Code Review 记录

![image-20200524095301632](http://nextcloud.hellochaos.cn/index.php/s/qXnDbgaMoF2zLfT/preview)

![image-20200523113726266](http://nextcloud.hellochaos.cn/index.php/s/PkQMbSBPWS4RdFA/preview)

![image-20200523204158731](http://nextcloud.hellochaos.cn/index.php/s/YBbQzWPe9LeNELE/preview)



### 3.3 issue内容和链接

| 团队成员 | issue内容和链接                                              |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | `#36`[改进定时发信功能的实现](https://github.com/gdut-very-good/island-drifting-backend/issues/36) |
| 黄煜淇   | `#39`[时间胶囊模块的开发](https://github.com/gdut-very-good/island-drifting-backend/issues/39) |
| 余圣源   | `#49`[完成头像上按钮样式修改](https://github.com/gdut-very-good/island-drifting-backend/issues/49) |
| 张文俊   | `#56`[完成海岛界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/56) |
| 丘丽珊   | `#63`[绘制第三天站立会议照片，<br/>一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/63) |
| 陈宇     | `#71`[用户在树洞下留言](https://github.com/gdut-very-good/island-drifting-backend/issues/71) |

## 四、最新模块截图

### 4.1 最新模块代码

```java
package com.verygood.island.task;

/**
 * @author <a href="mailto:kobe524348@gmail.com">黄钰朝</a>
 * @description 发信任务
 * @date 2020-05-23 10:20
 */

import com.verygood.island.entity.Letter;
import com.verygood.island.entity.Notice;
import com.verygood.island.entity.Stamp;
import com.verygood.island.entity.User;
import com.verygood.island.exception.bizException.BizException;
import com.verygood.island.mapper.LetterMapper;
import com.verygood.island.mapper.NoticeMapper;
import com.verygood.island.mapper.StampMapper;
import com.verygood.island.mapper.UserMapper;
import com.verygood.island.util.BeanUtils;
import lombok.extern.slf4j.Slf4j;

import java.time.LocalDateTime;

/**
 * 定时发送信件任务
 *
 * @author <a href="mailto:kobe524348@gmail.com">黄钰朝</a>
 * @date 2020-05-08
 */
@Slf4j
public class LetterSendingTask implements Runnable {

    /**
     * 要定时发送的信件
     */
    private final Letter letter;

    public LetterSendingTask() {
        this.letter = null;
    }

    public LetterSendingTask(Letter letter) {
        this.letter = letter;
    }

    @Override
    public void run() {
        //消耗邮票
        this.useStamp();
        UserMapper userMapper = BeanUtils.getBean(UserMapper.class);
        LetterMapper letterMapper = BeanUtils.getBean(LetterMapper.class);
        //发送信件
        if (null == letter) {
            log.warn("信件为空，无法执行发信任务");
            return;
        }
        letter.setReceiveTime(LocalDateTime.now());
        //统计接收到的信件数量
        User receiver = userMapper.selectById(letter.getReceiverId());
        receiver.setReceiveLetter(receiver.getReceiveLetter() + 1);
        userMapper.updateById(receiver);

        if (letterMapper.updateById(letter) == 1) {
            log.info("发送id为{}的letter成功，接收时间：{}", letter.getLetterId(), letter.getReceiveTime());
            //发送通知
            this.sendNotice();
        } else {
            log.error("发送id为{}的letter失败", letter.getLetterId());
            throw new BizException("发送失败[id=" + letter.getLetterId() + "]");
        }
    }

    /**
     * 使用邮票
     */
    private void useStamp() {
        StampMapper stampMapper = BeanUtils.getBean(StampMapper.class);
        Stamp stamp = new Stamp();
        stamp.setStampId(letter.getStampId());
        stamp.setUserId(letter.getReceiverId());
        stampMapper.updateById(stamp);
        log.info("使用id为{}的邮票成功", stamp.getStampId());
    }

    /**
     * 发送通知
     */
    private void sendNotice() {
        NoticeMapper noticeMapper = BeanUtils.getBean(NoticeMapper.class);
        UserMapper userMapper = BeanUtils.getBean(UserMapper.class);

        User sender = userMapper.selectById(letter.getSenderId());
        Notice notice = new Notice();
        notice.setTitle("收信通知");
        String content = "你收到一封来自" + sender.getNickname() + "的信件，快去查收吧！";
        notice.setContent(content);
        notice.setUserId(letter.getReceiverId());
        noticeMapper.insert(notice);
        log.info("发送notice成功，内容为：{}", content);
    }
}
```



### 4.2 程序运行截图

![image-20200524001554758](http://nextcloud.hellochaos.cn/index.php/s/YxaNs6PpPrqi2xB/preview)![image-20200524001621514](http://nextcloud.hellochaos.cn/index.php/s/JSjjzt5CncoiC48/preview)

## 五、每日总结

| 团队成员 | 总结内容                                                     |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | 今天比较忙，半天的时间要参与工作室招新的答辩，但也按时完成编码工作，很充实 |
| 黄煜淇   | 完善时间胶囊模块，参加了积极分子的讨论会和工作室的答辩       |
| 余圣源   | 要有合理的时间管理，分配好任务                               |
| 张文俊   | 合理分配时间                                                 |
| 丘丽珊   | 离终点又进步了一点点，开心!                                  |
| 陈宇     | commit时要仔细，不要将一些不必要的commit上去                 |



