---
layout: post
title: 被忽视了的NSDataDetector
date: 2018-7-30 18:40:59 +0300
description: 在Cocoa开发中，有一个简单的对于寻找数据的解决方案：NSDataDetector。 # Add post description (optional)
img: baishishan1.jpg  # Add image post (optional)
---

 
     done is better than perfect.
     
     - facebook 的办公室标语
     完成比完美更好
 
 > 机器只用二进制说话，而人类的语言却充满了谜语，真假，和省略。
 
 在日常开发场景中经常会遇到，在一段文本中检测一些半结构化的信息，比如：日期，地址，链接，电话号码和交通信息。
 如果这些需求在一个项目中出现，在不知道NSDataDetector这个类之前，可能要头皮发麻，之后开始自己编制一些正则，再加上国际化的需求，可能对编制好的正则需要大量的单元测试用例的介入。（估计好多小盆友要被这些东西整自闭了...）
 
 幸运的是，对于 Cocoa 开发者来说，有一个简单的解决方案：NSDataDetector。
 
 #### 关于NSDataDetector
 
 NSDataDetector 是 NSRegularExpression 的子类，而不只是一个 ICU 的模式匹配，它可以检测半结构化的信息：日期，地址，链接，电话号码和交通信息。
 
 它以惊人的准确度完成这一切。NSDataDetector 可以匹配航班号，地址段，奇怪的格式化了的数字，甚至是相对的指示语，如 “下周六五点”。
 
 你可以把它看成是一个有着复杂的令人难以置信的正则表达式匹配，可以从自然语言提取信息（尽管实际的实现细节可能比这个复杂得多）。
 
 NSDataDetector 对象用一个需要检查的信息的位掩码类型来初始化，然后传入一个需要匹配的字符串。像 NSRegularExpression 一样，在一个字符串中找到的每个匹配是用 NSTextCheckingResult 来表示的，它有诸如字符范围和匹配类型的详细信息。然而，NSDataDetector 的特定类型也可以包含元数据，如地址或日期组件。
 
 ```
 
 NSString * str = @"如果重置密码失败，400-800-2222请拨打400 500 5555客服热线：13800000000或者是(010)22222222还可以拨打+86 17700000000以及其他的号码比如：8617700000000以及177-0000-0000和其他的样式177 0000 0000";
 
 NSMutableAttributedString * attributr = [[NSMutableAttributedString alloc]initWithString:str];
 NSError *error = NULL;
 NSDataDetector *detector = [NSDataDetector dataDetectorWithTypes:NSTextCheckingTypePhoneNumber error:&error];
 NSRange inputRange = NSMakeRange(0, [str length]);
 NSArray *matches = [detector matchesInString:str options:0 range:inputRange];
 
 for (NSTextCheckingResult * match in matches) {
 if ([match resultType] == NSTextCheckingTypePhoneNumber){
 NSRange matchRange = [match range];
 
 }
 }
 
 ```
 > 当初始化 NSDataDetector 的时候，确保只指定你感兴趣的类型。每当增加一个需要检查的类型，随着而来的是不小的性能损失为代价。
 
 
 ### 数据检测器匹配类型
 
 NSDataDetector 的各种 NSTextCheckingTypes 匹配，及其相关属性表：
 
 | 类型                      | 属性                |
 | ------------------------- | ------------------- |
 | NSTextCheckingTypeDate    | 1. date             |
 |                           | 2. duration         |
 |                           | 3. timeZon          |
 | NSTextCheckingTypeAddress | 1.addressComponents |
 |                           |                     |

