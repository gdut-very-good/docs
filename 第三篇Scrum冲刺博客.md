[TOC]

Author：[歪瑞古德小队](https://www.cnblogs.com/misterchaos/p/12766888.html)

Project：[海岛漂流](https://www.cnblogs.com/misterchaos/p/12815587.html)

集合贴：[团队作业4：项目冲刺集合贴（歪瑞古德小队）](https://www.cnblogs.com/misterchaos/p/12934217.html)

## 一、Daily Scrum Meeting

### 1.1 会议照片

![image-20200524173829527](http://nextcloud.hellochaos.cn/index.php/s/SwtKeYDr3NNCnoo/preview)

### 1.2 项目进展

| 团队成员 | 昨日完成任务                                                 | 今日计划任务                                                 | 遇到的困难                                                   |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 黄钰朝   | `#34`[统计用户发送的信件<br>数量，接收的信件数量](https://github.com/gdut-very-good/island-drifting-backend/issues/34) | `#36`[改进定时发信功能的实现](https://github.com/gdut-very-good/island-drifting-backend/issues/36) | 配置xss过滤器时发现json<br>格式字符串被转义之后<br>无法被jackson解析 |
| 黄煜淇   | `#37`[完成自定义任务的编写](https://github.com/gdut-very-good/island-drifting-backend/issues/37) | `#39`[时间胶囊模块的开发](https://github.com/gdut-very-good/island-drifting-backend/issues/39) | redis无法反序列化非静态<br>内部类，将内部类提取出来          |
| 余圣源   | `#47`[完成消息界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/47) | `#49`[完成头像上按钮样式修改](https://github.com/gdut-very-good/island-drifting-backend/issues/49) | 尝试打包安卓应用，出现<br>错误，至今原因未明                 |
| 张文俊   | `#55`[完成海岛列表界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/55) | `#56`[完成海岛界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/56) | 配安卓环境有困难                                             |
| 丘丽珊   | `#62`[绘制第二天站立会议照片](https://github.com/gdut-very-good/island-drifting-backend/issues/62) | `#63`[绘制第三天站立会议照片，<br/>一张信纸样式](https://github.com/gdut-very-good/island-drifting-backend/issues/63) | 难以保证工作同步的<br>时效性，将努力在周末<br>时间跟上消息更替速度；<br>作业好多，时间好少，<br>课程好多。 |
| 陈宇     | `#70`[用户删除已经创建的树洞](https://github.com/gdut-very-good/island-drifting-backend/issues/70) | `#71`[用户在树洞下留言](https://github.com/gdut-very-good/island-drifting-backend/issues/71) | mybatisplus分页不太熟悉                                      |

## 二、项目燃尽图

![05-23燃尽图](http://nextcloud.hellochaos.cn/index.php/s/TnEL2wCDEC3S7D8/preview)

## 三、签入记录

### 3.1 代码/文档签入记录

![image-20200523093923263](http://nextcloud.hellochaos.cn/index.php/s/oQMH2ATLEdCXGPN/preview)

### 3.2 Code Review 记录

![image-20200523094021843](http://nextcloud.hellochaos.cn/index.php/s/QxeJZ4yNadjLHNp/preview)

![image-20200523094119588](http://nextcloud.hellochaos.cn/index.php/s/qzTCDcaZ6oksb9K/preview)

![image-20200523094200242](http://nextcloud.hellochaos.cn/index.php/s/4BGEesnzsdwmZNN/preview)

### 3.3 issue内容和链接

| 团队成员 | issue内容和链接                                              |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | `#34`[统计用户发送的信件<br>数量，接收的信件数量](https://github.com/gdut-very-good/island-drifting-backend/issues/34) |
| 黄煜淇   | `#37`[完成自定义任务的编写](https://github.com/gdut-very-good/island-drifting-backend/issues/37) |
| 余圣源   | `#47`[完成消息界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/47) |
| 张文俊   | `#55`[完成海岛列表界面和功能](https://github.com/gdut-very-good/island-drifting-backend/issues/55) |
| 丘丽珊   | `#62`[绘制第二天站立会议照片](https://github.com/gdut-very-good/island-drifting-backend/issues/62) |
| 陈宇     | `#70`[用户删除已经创建的树洞](https://github.com/gdut-very-good/island-drifting-backend/issues/70) |

## 四、最新模块截图

### 4.1 最新模块代码

**（1）全局异常处理器**

```java
/**
 * @author <a href="mailto:kobe524348@gmail.com">黄钰朝</a>
 * @description 全局异常处理器
 * @date 2019-08-12 19:19
 */
@Slf4j
@RestControllerAdvice
@CrossOrigin
public class GlobalExceptionHandler implements HandlerExceptionResolver {

    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest,
                                         HttpServletResponse httpServletResponse,
                                         Object o, Exception e) {
        log.info("请求异常" + e.getMessage());
        e.printStackTrace();
        return null;
    }

    @ExceptionHandler(com.verygood.island.exception.bizException.BizException.class)
    public ResultBean<?> bizException(BizException e) {
        return new ResultBean<>(e);
    }

    @ExceptionHandler(org.apache.shiro.authc.AuthenticationException.class)
    public ResultBean<?> authenticationException(Exception e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException(e.getMessage()));
    }

    @ExceptionHandler(NullPointerException.class)
    public ResultBean<?> nullPointerException(NullPointerException e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("错误!参数不匹配"));
    }

    @ExceptionHandler({com.alibaba.druid.pool.GetConnectionTimeoutException.class})
    public ResultBean<?> dataBaseException(Exception e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("服务器访问人数过多，请稍后重试"));
    }


    @ExceptionHandler(
            org.springframework.web.multipart.MaxUploadSizeExceededException.class)
    public ResultBean<?> maxUploadexception(
            org.springframework.web.multipart.MaxUploadSizeExceededException e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("您上传的文件大小超过限制"));
    }

    @ExceptionHandler(Throwable.class)
    public ResultBean<?> unknownException(Throwable e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("发生了未知的异常，请告知程序员哥哥前来修复"));
    }

    @ExceptionHandler(
            {org.springframework.web.method.annotation.MethodArgumentTypeMismatchException.class,
                    org.springframework.http.converter.HttpMessageNotReadableException.class,
                    org.springframework.web.bind.MissingServletRequestParameterException.class})
    public ResultBean<?> http400Handler(Exception e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("您的请求错误，缺少请求体或格式错误"));
    }

    @ExceptionHandler(
            org.springframework.web.HttpRequestMethodNotSupportedException.class)
    public ResultBean<?> http405Handler(
            org.springframework.web.HttpRequestMethodNotSupportedException e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("服务器并不支持您所使用的请求方法"));
    }

    @ExceptionHandler(
            org.springframework.web.HttpMediaTypeNotSupportedException.class)
    public ResultBean<?> http405Handler(
            org.springframework.web.HttpMediaTypeNotSupportedException e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("您的请求体格式不正确"));
    }


    @ExceptionHandler(IllegalStateException.class)
    public ResultBean<?> http500Handler(IllegalStateException e) {
        e.printStackTrace();
        return new ResultBean<>(new BizException("您的请求体格式不正确"));
    }

}

```

（2）**定时任务**

```java
/**
 * @author huange7
 */
@Slf4j
public class CapsuleSendingTask implements Runnable {

    /**
     * 存储时间胶囊
     */
    private final Letter letter;

    public CapsuleSendingTask() {
        letter = new Letter();
    }

    /**
     * 构造器
     *
     * @param letter 发送的信件
     */
    public CapsuleSendingTask(Letter letter) {
        this.letter = letter;
    }

    @Override
    public void run() {
        // 时间胶囊数量减1
        reduceCapsule();

        LetterServiceImpl letterService = BeanUtils.getBean(LetterServiceImpl.class);
        letter.setReceiveTime(LocalDateTime.now());

        if (letterService.updateById(letter)) {
            log.info("发送id为{}的letter成功，接收时间：{}", letter.getLetterId(), letter.getReceiveTime());
            //发送通知
            this.sendNotice();
        } else {
            log.error("发送id为{}的letter失败", letter.getLetterId());
            throw new BizException("发送失败[id=" + letter.getLetterId() + "]");
        }
    }

    /**
     * 发送通知
     */
    private void sendNotice() {
        Notice notice = new Notice();
        NoticeMapper noticeMapper = BeanUtils.getBean(NoticeMapper.class);
        notice.setTitle("时间胶囊通知");
        String content = "你收到一个来自自己的时间胶囊，快去查收吧！";
        notice.setContent(content);
        notice.setUserId(letter.getReceiverId());
        noticeMapper.insert(notice);
        log.info("发送notice成功，内容为{}", content);
    }

    /**
     * 减去对应的时间胶囊
     */
    private void reduceCapsule() {
        UserMapper userMapper = BeanUtils.getBean(UserMapper.class);
        User user = userMapper.selectById(letter.getSenderId());
        if (user == null) {
            log.info("减去时间胶囊时发送错误！不存在该用户");
            return;
        }
        if (user.getCapsule() <= 0) {
            log.info("减去时间胶囊时发送错误！胶囊数量不足");
            return;
        }
        user.setCapsule(user.getCapsule() - 1);
        userMapper.updateById(user);
    }

    @Override
    public String toString() {
        return "CapsuleSendingTask{" +
                "letter=" + letter +
                '}';
    }
}
```

**（3）删除树洞**

```java
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
```

### 4.2 程序运行截图

![image-20200523093701486](http://nextcloud.hellochaos.cn/index.php/s/MR2Q5GSZXZDrb6B/preview)![image-20200523175631562](http://nextcloud.hellochaos.cn/index.php/s/Xzdkq34FgNZfcf5/preview)

## 五、每日总结

| 团队成员 | 总结内容                                                     |
| -------- | ------------------------------------------------------------ |
| 黄钰朝   | 加深了对于springboot中filter,wrapper以及exceptionHandler的使用 |
| 黄煜淇   | redis反序列化时需要注意补充无参构造方法                      |
| 余圣源   | 以后尽量避免跨平台开发复杂应用                               |
| 张文俊   | 下次要找个网络好的地方下载                                   |
| 丘丽珊   | 我不知道画个图有啥总结...                                    |
| 陈宇     | 学习mybatisplus的分页                                        |



