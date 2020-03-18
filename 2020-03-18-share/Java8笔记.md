### java8学习笔记


##### java遍历map的方式：
    1、 map.keySet().forEach(key -> System.out.println("map.get(" + key + ") = " + map.get(key)));通过Map.keySet遍历key和value
    2、map.entrySet().iterator().forEachRemaining(item -> System.out.println("key:value=" + item.getKey() + ":" + item.getValue()));通过Map.entrySet使用Iterator遍历key和value
    3、map.entrySet().forEach(entry -> System.out.println("key:value = " + entry.getKey() + ":" + entry.getValue()));通过Map.entrySet遍历key和value，在大容量时推荐使用
    4、map.values().forEach(System.out::println); // 等价于map.values().forEach(value -> System.out.println(value));通过Map.values()遍历所有的value，但不能遍历key
    5、map.forEach((k, v) -> System.out.println("key:value = " + k + ":" + v));通过k,v遍历，Java8独有的



##### 排序：java8  列出班上超过85分的学生姓名，并按照分数降序输出用户名字
    public void test1() {
        List<String> studentList = stuList.stream()//stuList是一个封装对象的list
                .filter(x->x.getScore()>85)
                .sorted(Comparator.comparing(Student::getScore).reversed())
                .map(Student::getName)
                .collect(Collectors.toList());
        System.out.println(studentList);
    }
    给java.util.Collection接口添加新方法，如stream()、parallelStream()、forEach()和removeIf()等等。



##### java8比较器：
    (1)、public void testSortByName_with_lambda() throws Exception {
       ArrayList<Human> humans = Lists.newArrayList(
               new Human("tomy", 22),
               new Human("li", 25)
       );
       humans.sort((Human h1, Human h2) -> h1.getName().compareTo(h2.getName()));
       humans.sort(Comparator.comparing(Human::getName).thenComparing(Human::getAge));
    }
    
    (2)、java8流式排序：
    List<Student> studentList3=studentList.stream().sorted(Comparator.comparing(Student::getAge)).collect(Collectors.toList());//根据年龄自然顺序
    
    (3)、java8组合排序：先比较年龄，年龄相同再比较名称  提取Comparator
    Collectons.sort(list,Comparator.comparing(User::getAge).thenComparing(User::getName));
    List<User> studentList4 = userList.stream().sorted(Comparator.comparing(User::getAge).reversed().thenComparing(User::getId).reversed()).collect(Collectors.toList());
    
    (4) 多条件排序
        list.sort((l, r) -> {
        // 导入com.ibm.icu.text.Collator包 先排中文字符，再排英文字符
         Collator collatorChinese = Collator.getInstance(java.util.Locale.CHINA);     
         if (l.getName().equals(r.getName())) {
              return l.getAge() - r.getAge();
         } else {
          collatorChinese.compare(l.getName(), r.getName());     
         }
        });
        或：List<User> studentList3=userList.stream().filter(user -> user.getName() != null && user.getAge() != null).sorted((x, y) -> {
                if (x.getName().equals(y.getName())) {
                    return x.getAge() - y.getAge();
                } else {
                    return x.getName().compareTo(y.getName());
                }
            }).collect(Collectors.toList()); // 流式排序，在之前可以加过滤
        



##### lamda表达式把list转Map 
    List<User> userLists = new ArrayList();
    Map<String, User> userMap = userLists.stream().collect(Collectors.toMap(User::getId, userList -> userList, (oldValue, newValue)->newValue));
    或：Map<String, User> userMap = userLists.stream().collect(Collectors.toMap(User::getId, Function.identity()));


#####  apache的StringUtils类下面常用的的方法：
    1、public static boolean isEmpty(String str)  字符串判空
    
    2、public static boolean isNotEmpty(String str)  判断某字符串是否非空
    
    3、public static boolean isBlank(String str)  判断某字符串是否为空或长度为0或由空白符(whitespace) 构成（可判断空格符）
    
    4、public static boolean isNotBlank(String str)  判断某字符串是否不为空且长度不为0且不由空白符(whitespace) 构成
    
    5、public static String trim(String str)   去掉字符串两端的控制符(control characters, char <= 32) , 如果输入为 null 则返回null  如/t /f whitespace 等制表符、换行符、换页符和回车符
    
    6、public static String trimToNull(String str)  去掉字符串两端的控制符(control characters, char <= 32) ,如果变为 null 或""，则返回 null
    
    7、public static String trimToEmpty(String str)  去掉字符串两端的控制符(control characters, char <= 32) ,如果变为 null 或 "" ，则返回 ""
    
    8、public static boolean equals(String str1, String str2)   比较两个字符串是否相等，如果两个均为空则也认为相等。
    
    9、public static boolean equalsIgnoreCase(String str1, String str2) 比较两个字符串是否相等，不区分大小写，如果两个均为空则也认为相等。
    
    10、public static int indexOf(String str, char searchChar, int startPos) 返回字符 searchChar 从 startPos 开始在字符串 str 中第一次出现的位置。如果从 startPos 开始 searchChar 没有在 str 中出现则返回-1，如果 str 为 null 或 "" ，则也返回-1。
    
    11、public static int indexOf(String str, String searchStr)  返回字符串 searchStr 在字符串 str 中第一次出现的位置。如果 str 为 null 或 searchStr 为 null 则返回-1，如果 searchStr 为 "" ,且 str 为不为 null ，则返回0，如果 searchStr 不在 str 中，则返回-1
    
    12、public static int ordinalIndexOf(String str, String searchStr, int ordinal)   返回字符串 searchStr 在字符串 str 中第 ordinal 次出现的位置。
    
    13、public static <T> String join(T... elements) 将返回直接首尾拼接的字符串
    



##### Java8的流式编程：
    1、Stream 提供了新的方法 'forEach' 来迭代流中的每个数据
    
    2、map 方法用于映射每个元素到对应的结果
    
    3、filter 方法用于通过设置的条件过滤出元素
    
    4、limit 方法用于获取指定数量的流  random.ints().limit(10).sorted().forEach(System.out::println);   使用 sorted 方法对输出的 10 个随机数进行排序
    
    5、parallelStream 是流并行处理程序的代替方法
    
    6、Collectors 类实现了很多归约操作，例如将流转换成集合和聚合元素
    
    7、distinct()  去重  userList = userList.stream().distinct().collect(Collectors.toList()); //重写User的equal和hashcode方法，String和基本类型可直接使用
    
    8、Arrays.stream(String[] array)....
  
##### java8对时间上的处理：
    1、LocalDateTime currentTime = LocalDateTime.now();  // 获取当前的日期时间   输出：当前时间: 2016-04-15T16:55:48.668
    2、Month month = currentTime.getMonth();
      int day = currentTime.getDayOfMonth();
      int seconds = currentTime.getSecond();
      System.out.println("月: " + month +", 日: " + day +", 秒: " + seconds);
      输出：月: APRIL, 日: 15, 秒: 48
      
    3、 LocalDate date3 = LocalDate.of(2014, Month.DECEMBER, 12); 或者LocalDate date = LocalDate.of(2014, 12, 12);
      System.out.println("date3: " + date3);
      输出：date3: 2014-12-12
      
    4、各时间的转换关系
      一、LocalDate -> String   如果时间格式为LocalDate  则： LocalDate local = LocalDate.Now(); String date = local.format(DateTimeFormatter.ofPattern("yyyy-MM-dd"));
          String -> LocalDate or LocalDateTime:
          String beginDate = "2018年12月09";
          LocalDateTime beginDateTime = LocalDateTime.parse(beginDate, DateTimeFormatter.ofPattern("yyyy年M月d"));
          输出：2018-12-09
          
      一、Date -> LocalDateTimeDate  or LocalDate or LocalTime
        date = new Date();
        LocalDateTime local = date.t oInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
        或：Date date = new Date(); 
            Instant instant = date.toInstant();
            ZoneId zone = ZoneId.systemDefault();
            LocalDateTime localDateTime = LocalDateTime.ofInstant(instant, zone);
            
      二、LocalDateTimeDate -> Date
             Date dates = Date.from(local.atZone(ZoneId.systemDefault()).toInstant());
             
      三、LocalDate -> Date
            LocalDate localDate = LocalDate.now();
            ZoneId zone = ZoneId.systemDefault();
            Instant instant = localDate.atStartOfDay().atZone(zone).toInstant();
            java.util.Date date = Date.from(instant);
            
      四、LocalDateTime -> LocalDate
              LocalDateTime local = LocalDateTime.now();
              LocalDate locals = local.toLocalDate();
              
      五、LocalDate-> LocalDateTime
              LocalDate lo = LocalDate.now();
              LocalDateTime lo = lo.atStartOfDay();
              
      六、LocalDateTime->LocalTime
              LocalDateTime local = LocalDateTime.now();
              LocalTime time = local.toLocalTime();
    
       5、plusMonths(long num) 返回目前月份的下num个月 例：String nextMonth = LocalDate.Now().PlusMonths(1).format(DateTimeFormatter.ofPattern("yyyy-MM"));
       
       6、获取年、月、日信息
          LocalDate now = LocalDate.now();
          int year = now.getYear();
          int monthValue = now.getMonthValue();
          int dayOfMonth = now.getDayOfMonth();
          结果是now = 2018-06-20  year = 2018, month = 6, day = 20
          
       7、判断两个日期是否相等
            LocalDate now = LocalDate.now();
            LocalDate date = LocalDate.of(2018, 06, 20);
            if (date.equals(now)) {
                System.out.println("同一天");
            }
            
    8、检查像生日这种周期性事件
        LocalDate now = LocalDate.now();
        LocalDate dateOfBirth = LocalDate.of(2018, 06, 20);
        MonthDay birthday = MonthDay.of(dateOfBirth.getMonth(), dateOfBirth.getDayOfMonth());
        MonthDay currentMonthDay = MonthDay.from(now);
        if (currentMonthDay.equals(birthday)) {
            System.out.println("Happy Birthday");
        } else {
            System.out.println("Sorry, today is not your birthday");
        }
        
    9、获取当前时间
      LocalTime localTime = LocalTime.now();
      System.out.println(localTime);
      结果：13:35:56.155
      
    10、在现有的时间上增加小时
        LocalTime localTime = LocalTime.now();
        System.out.println(localTime);
        LocalTime localTime1 = localTime.plusHours(2);//增加2小时
        System.out.println(localTime1);
        输出：09:09:10.840
              11:09:10.840
              07:09:10.840
              
    11、计算一周后的日期   可以用同样的方法增加 1 个月、1 年、1 小时、1 分钟甚至一个世纪，更多选项可以查看 Java 8 API 中的 ChronoUnit 类
        LocalDate now = LocalDate.now();
        LocalDate plusDate = now.plus(1, ChronoUnit.WEEKS);
        LocalDate minusDate = now.minus(1, ChronoUnit.YEARS);
        System.out.println(now);
        System.out.println(plusDate);
        System.out.println(minusDate);
        输出：2018-06-20
              2018-06-27
              2017-06-27
              
    12、如何用 Java 判断日期是早于还是晚于另一个日期
        LocalDate 类有两类方法 isBefore() 和 isAfter() 用于比较日期。调用 isBefore() 方法时，如果给定日期小于当前日期则返回 true。
        LocalDate tomorrow = LocalDate.of(2018,6,20);
        // 不包含now对应的日期
        if(tomorrow.isAfter(now)){
            System.out.println("Tomorrow comes after today");
        }
        LocalDate yesterday = now.minus(1, ChronoUnit.DAYS);
        if(yesterday.isBefore(now)){
            System.out.println("Yesterday is day before today");
        }
        
    13、获取年月和天数
        YearMonth currentYearMonth = YearMonth.now();
        System.out.printf("Days in month year %s: %d%n", currentYearMonth, currentYearMonth.lengthOfMonth());
        YearMonth creditCardExpiry = YearMonth.of(2018, Month.FEBRUARY);
        System.out.printf("Your credit card expires on %s %n", creditCardExpiry);
        输出：Days in month year 2018-06: 30
              Your credit card expires on 2018-02
              
    14、如何在 Java 8 中检查闰年
        LocalDate 类有一个很实用的方法 isLeapYear() 判断该实例是否是一个闰年。
        
    15、计算两个日期之间的天数
        LocalDate date = LocalDate.of(2018, 12, 20);
        LocalDate now = LocalDate.now();
        Period period = Period.between(now, date);
        System.out.println("离下个时间还有" + period.getDays() + "天");
        输出：离下个时间还有20 个月
        
    16、在 Java 8 中获取当前的时间戳
        Instant timestamp = Instant.now();
        System.out.println(timestamp);
        输出：2018-06-20T06:35:24.881Z

   
##### 过滤集合转map
    sellTradeDetailsMap = sellTradeDetails.stream().filter(tradeDetali -> unitMap.containsKey(tradeKey(tradeDetali.getUnitId())).collect(Collectors.toMap(elecAllocate -> {return "key";},elecAllocate -> elecAllocate));
   
  
##### Collectors.toMap 集合转map
    tradeStrategiseMap = tradeStrategies.stream().collect(Collectors.toMap(TradeStrategyType :: getTypeCode -> {}, typeVo -> typeVo));
    或：tradeStrategiseMap = tradeStrategies.stream().collect(Collectors.toMap(Emp::getId, Emp::getName);
    或：Map<String,List<Sales>> trendMap = salesList.stream().collect(Collectors.groupingBy(
                s -> s.getStDate() + "_" + s.getPlatform() + "_" + s.getCategoryId()));
    
  
##### stream().map和stream().peek
    List<User> userList = dataList.stream().map(data -> {
    User user = new User();
    user.setName(data.getName());
    user.setAge(data.getAge());
    return age;
    }).collect(Collectors.toList());
    把一个集合中的数据转换成另外一个集合中的数据  map 方法用于映射每个元素到对应的结果  一个实体 -> 另外一个实体
        
    List<User> userList = dataList.stream().peek(data -> {
    data.setName("zhangsan");
        data.setAge(0);
    }).collect(Collectors.toList()); 
    List<User> userList = dataList.stream().peek(data -> {
        data.setName("zhangsan");
        data.setAge(0);
    }).collect(Collectors.toMap(User::getName, User::getAge)); //可以先set在直接获取，免得再tomap里面在写复杂lamda
    peek方法比map方法少了return  主要对遍历的元素进行操作   一个实体 -> 同一个实体

##### java8流的方法：
        1、findFirst() 返回一个流中第一个集合对象，可以通过get()方法获取它的内容
        
        2、findAny() 串行流中和getfirst方法一样，并行流中返回最快出理完线程的数据 同样可以通过get()方法获取值
        例：Optional<String> aa = strs.stream().filter(str -> !str.equals("a")).findFirst();  strs也是List<Optional<String>>
            
        3、max() 返回集合中最大的元素
        
        4、min() 返回集合中最小的元素
        Optional<Integer> squaresList = numbers.stream().filter(nu -> nu > 1).max(Comparator.comparing(artist -> artist));  numbers是Integer集合
        
        5、limit(N) 截取流的前N个元素
        
        6、count() 统计集合的个数返回int
        
        7、skip(N)  跳过流的前N个元素
        
        8、distinct() 去除集合中重复的元素
        
        10、flatmap()  可以在lamda中返回集合，然后flat为单个元素一个个放入最后的结果集中。List<Integer> result = outer.stream().flatMap(p -> p.getHobbyLists.stream().map(Hobby :: getHobby)).collect(toList()); //outer里面有一个List<Integer> hobbyList
        
        11、List<Person> personList2 = persons.stream().limit(2).sorted((p1, p2) -> p1.getName().compareTo(p2.getName())).collect(Collectors.toList());
        
        12、int count = Stream.of(1,2,3).reduce(0,(acc,element) -> acc + element);
        
        0 + 1 = 1  1 + 2 = 3  3 + 3 = 6   0代表给acc给的初始值 element代表集合的元素
        
        BigDecimal acc = numbers.stream().reduce(BigDecimal.valueOf(1), (x, y) -> y.add(y));  //numbers是List<BigDecimal>
        
        String sd = strs.stream().map(User::getId).collect(Collectors.toList()).stream().reduce("", (x, y) -> x.concat(y));  把数组strs中的User中String类型的id连接起来
        
        BigDecimal acc = strs.stream().filter(u -> u.getMoney != null).map(User::getMoney).reduce(BigDecimal.ZERO, BigDecimal::add); // 推荐
        
        Optional<Sales> optional = childList.stream().reduce((a,b) -> {
                    a.setMoney(Optional.ofNullable(a.getMoney()).orElse(BigDecimal.ZERO).add(Optional.ofNullable(b.getMoney()).orElse(BigDecimal.ZERO)));
                    a.setAge(Optional.ofNullable(a.getAge()).orElse(0) + Optional.ofNullable(b.getAge()).orElse(0));
                    return a;
                }); // 求价格和，年龄和
        // 字符串连接，concat = "ABCD"
        String concat = Stream.of("A", "B", "C", "D").reduce("", String::concat);
        
        13、是否匹配任一元素：anyMatch
        boolean result = list.stream().anyMatch(Person::isStudent);//判断list中是否有学生
        
        14、是否匹配所有元素：allMatch
        boolean result = list.stream().allMatch(Person::isStudent);//判断是否所有人都是学生
        
        15、是否未匹配所有元素：noneMatch，与allMatch相反
        boolean result = list.stream().noneMatch(Person::isStudent);
        
        16、获取任一元素findAny
        Optional<Person> person = list.stream().findAny();
        



##### java8中的Optional  目的：防止空指针异常
    Optional 的三种构造方式: Optional.of(obj),  Optional.ofNullable(obj) 和 Optional.empty()
    方法：
    
    1、static <T> Optional<T> empty()  返回空的 Optional 实例。
    
    2、boolean equals(Object obj) 判断其他对象是否等于 Optional。
    
    3、T get()  如果在这个Optional中包含这个值，返回值，否则抛出异常：NoSuchElementException
    
    4、void ifPresent(Consumer<? super T> consumer)   如果值存在则使用该值调用 consumer , 否则不做任何事情。
    
    5、boolean isPresent()   如果值存在则方法会返回true，否则返回 false。
    
    6、static <T> Optional<T> of(T value)    返回一个指定非null值的Optional，如果传入为空则报异常
    
    7、T orElse(T other)   如果存在该值，返回值， 否则返回 other。
    
    8、T orElseGet(Supplier<? extends T> other)  如果存在该值，返回值， 否则触发 other，并返回 other 调用的结果。 例：return user.orElseGet(() -> fetchAUserFromDatabase()); 没有就出发后面的函数
    
    9、static <T> Optional<T> ofNullable(T value)    如果为非空，返回 Optional 描述的指定值，否则返回空的 Optional，传 null 进到就得到 Optional.empty()。（建议用这个不用of）
    
    10、<U>Optional<U> map(Function<? super T,? extends U> mapper)  
       如果有值，则对其执行调用映射函数得到返回值。如果返回值不为 null，则创建包含映射返回值的Optional作为map方法返回值，否则返回空Optional。
       
    11、Optional<T> filter(Predicate<? super <T> predicate)
      如果值存在，并且这个值匹配给定的 predicate，返回一个Optional用以描述这个值，否则返回一个空的Optional。其中Predicate<? super <T> predicate可以用User::valid 来代替
    
    例1：user.ifPresent(System.out::println); 存在则对他做些事情，不存在则什么都不做，括号里面可以写lamda表达式 
    //等同于 ：
    if (user.isPresent()) {
      System.out.println(user.get());
    }
    
    例2： 以下代码不会报空指针异常
        List<OptionalTest> s1 = new ArrayList<>();
        OptionalTest test = new OptionalTest();
        s1.add(test);
        Optional<OptionalTest> name3 = Optional.ofNullable(test); //如果test为空，返回空的 Optional 不为空返回指定值
        String sd = null;
        Optional<String> name2 = Optional.ofNullable(sd); // 建议用ofNullable而不是of 不然传入值为空也会报空指针异常
        Optional<String> s = name2.map((value) -> value.toUpperCase());
        System.out.println(s.orElse("No value found"));//No value found
        等同于：
        return user.map(u -> u.getUsername())
           .map(name -> name.toUpperCase())
           .orElse(null);  // 不会报空指针异常
    
    例3：把name大写，不会报空指针异常
    User user = new User();
    user.setName(null);
    Optional<User> op = Optional.ofNullable(user);
    user.setName(op.map(u -> u.getName())
                .map(name -> name.toUpperCase())
                .orElse(null));
    System.out.println(user);
    
    例4：最终方案：
        User user = new User();
        user.setName(null);
        user.setId("aa");
        user.setName(Optional.ofNullable(user.getName()).map(s -> s.toUpperCase()).orElse(null));
        user.setId(Optional.ofNullable(user.getId()).map(s -> s.toUpperCase()).orElseThrow(() -> new Exception()));
        或：user.setId(Optional.ofNullable(user.getId().toUpperCase()).orElseThrow(() -> new Exception())); // id为null也不会报空指针异常
        
		
##### Java8中String.join方法，底层为StringJoiner，StringJoiner 的底层实现就是 StringBuilder
    List<String> list = new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");
    list.add("e");
    String.join(";", list);
    输出：a,b,c,d,e  //注意首尾都无，
    也可以这样：
    List<String> list = new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");
    list.add("e");
    String s = list.stream().collect(Collectors.joining(",", "{", "}"));
    输出：{a,b,c,d,e}

##### Java8 中Collectors类的常用的静态工厂方法：
    工厂方法         返回类型	                         用途                                         使用示例:
    1、toList           List<T>	             把流中所有项目收集到一个 List                                List<Dish> dishes = menuList.stream().collect(Collectors.toList());
    
    2、toSet            Set<T>	             把流中所有项目收集到一个 Set，删除重复项                     List<Dish> dishes = menuList.stream().collect(Collectors.toSet());
    
    3、toCollection     Collection<T>        把流中所有项目收集到给定的供应源创建的集合                   Collection<Dish> dishes = menuStream.collect(toCollection(), ArrayList::new);
    
    4、summingInt       Integer	             对流中项目的一个整数属性求和                                 int totalCalories = menuStream.collect(Collectors.summingInt(Dish::getCalories));
    
    5、averagingInt     Double               计算流中项目 Integer 属性的平均值                            double avgCalories =menuStream.collect(Collectors.averagingInt(Dish::getCalories));
    
    6、summarizingInt   IntSummaryStatistics 收集关于流中项目 Integer 属性的统计值，例如最大、最小、总和与平均值  ntSummaryStatistics menuStatistics =menuStream.collect(Collectors.summarizingInt(Dish::getCalories));
    
    7、joining          String               连接对流中每个项目调用 toString 方法所生成的字符串            String shortMenu =menuStream.map(Dish::getName).collect(Collectors.joining(", ")); 或Collectors.joining(",", "{", "}")
    
    8、maxBy            Optionnal<T>         一个包裹了流中按照给定比较器选出的最大元素的 Optional，或如果流为空则为 Optional.empty()   Optional<Dish> fattest =menuStream.collect(Collectors.maxBy(comparingInt(Dish::getCalories)));   或Optional<User> fattest =userList.stream().collect(Collectors.maxBy((x, y) -> x.getAge() - y.getAge())); 获取：System.out.println(fattest.map(u -> u).orElse(new User()));
    
    9、minBy            Optional<T>          一个包裹了流中按照给定比较器选出的最小元素的 Optional，或如果流为空则为 Optional.empty()   Optional<Dish> fattest =menuStream.collect(Collectors.minBy(comparingInt(Dish::getCalories)));   或tasks.stream().collect(collectingAndThen(maxBy((t1, t2) -> t1.getTitle().length() - t2.getTitle().length()), Optional::get));
    
    10、reducing       归约操作产生的类型    从一个作为累加器的初始值开始，利用 BinaryOperator 与流 中的元素逐个结合，从而将流归约为单个值   int totalCalories =menuStream.collect(Collectors.reducing(0, Dish::getCalories, Integer::sum));  或把Integer::sum替换成BigDecimal::add
    
    11、collectingAndThen  转换函数返回类型   包裹另一个收集器，对其结果应用转换函数                       int howManyDishes =menuStream.collect(Collectors.collectingAndThen(toList(), List::size));
    
    12、groupingBy      Map<K,List<T>>       根据项目的一个属性的值对流中的项目作问组，并将属性值作 为结果 Map 的键   Map<Dish.Type,List<Dish>> dishesByType =menuStream.collect(Collectors.groupingBy(Dish::getType));
        或 List<Student> list1
           Map<Student,Long> result2=list1.stream().collect(Collectors.groupingBy(Function.identity(),Collectors.counting()));
           分组，并统计其中一个属性值得sum或者avg:id总和
            Map<String,Integer> result3=list1.stream().collect(
                    Collectors.groupingBy(Student::getGroupId,Collectors.summingInt(Student::getId))
            );
            Map<String, Double> mapn = userList.stream().collect(Collectors.groupingBy(User::getName, Collectors.summingDouble(u -> u.getMoney().doubleValue()))); //求和，如果money是BigDecimal，注意如果money存在小数点后12位以上，不能采取这种方式去求和，应该采取下面这种方式：
            Map<String, BigDecimal> mapn = userList.stream().collect(Collectors.groupingBy(User::getName, Collectors.reducing(BigDecimal.ZERO, User::getMoney, BigDecimal::add)));
        或：Map<String, List<Integer>> groupMap = fruitList.stream().collect(Collectors.groupingBy(Fruit::getName, Collectors.mapping(Fruit::getPrice, Collectors.toList()))); //如苹果有三个档次的价格（重要）
        或：
        List<String> list = new ArrayList<>();
        Map<String, Long> map = list.stream().collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        int sd = map.getOrDefault("a", new Long(0)).intValue();
        
    13、partitioningBy  Map<Boolean,List<T>>  根据对流中每个项目应用谓词的结果来对项目进行分区             Map<Boolean,List<Dish>> vegetarianDishes =menuStream.collect(Collectors.partitioningBy(Dish::getType));





##### java8新增的常用的Map方法：
    1. V getOrDefault(key,defaultValue):获取key值,如果key不存在则用defaultValue
           map.getOrDefault(3,"val_66")
           
    2. V putIfAbsent(K key, V value):根据key匹配Node,如果匹配不到则增加key-value,返回null,如果匹配到Node,如果oldValue不等于null则不进行value覆盖，返回oldValue
        map.put("123", "456");
        System.out.println(map.putIfAbsent("1", "34"));// null
        System.out.println(map.putIfAbsent("123", "sss"));//456
        
    3. boolean remove(Object key, Object value):根据key匹配node，如果value也相同则删除
    
    4. boolean replace(K key, V oldValue, V newValue):根据key匹配node,如果value也相同则使用newValue覆盖返回true，否则返回false
        map.put("123", "456");
        System.out.println(map.replace("123", "456", "789"));
        System.out.println(map.get("123"));
        
    5. V compute(K key,BiFunction remappingFunction)：根据key做匹配，根据BiFunction的apply返回做存储的value,
       匹配到Node做value替换，匹配不到新增node。apply的返回值如果为null则删除该节点，否则即为要存储的value。
       System.out.println(map.compute(10,(key,value) -> {return value.split("_")[1];}));//666666 -》用返回值覆盖原来的值
       System.out.println(map.compute(6,(key,value) ->  null));//null -》返回值为null，则删除该key值
       
    6. * merge(K key, V value,BiFunctionsuper V, ? super V, ? extends V> remappingFunction):
         * 功能大部分与compute相同，不同之处在于BiFunction中apply的参数，入参为oldValue、value，
         * 调用merge时根据两个value进行逻辑处理并返回value。
        System.out.println(map.merge(3,"val_3",(value,newValue) -> newValue));//val_3  --》返回值覆盖原来的value
        System.out.println(map.merge(10,"33334",(a,b) -> (Integer.valueOf(a)+Integer.valueOf(b))+""));//700000
        System.out.println(map.merge(8,"88",(oldValue,newValue) -> oldValue+newValue));//88 -》key不存在则新增
        System.out.println(map.merge(11,"11",(old,newValue) -> null));//null -》返回值为null,删除该节点
        System.out.println(map.toString());//{0=val_0, 1=val_1, 2=val_2, 3=val_3, 4=val_4, 5=val_5, 8=88, 10=700000}
        
    7. computeIfAbsent(K key,Functionsuper K, ? extends V> mappingFunction):
         * 根据key做匹配Node，（匹配不到则新建然后重排）
         * 如果Node有value，则直接返回oldValue，
         * 如果没有value则根据Function接口的apply方法获取value，返回value。
         * Function接口的apply的入参为key，调用computeIfAbsent时重写Function接口可以根据key进行逻辑处理，
         * apply的返回值即为要存储的value。
          map.put(8,null);
        System.out.println(map.toString());//{0=val_0, 1=val_1, 2=val_2, 3=val_3, 4=val_4, 5=val_5, 8=null, 10=700000}
        System.out.println(map.computeIfAbsent(0,key -> key+"000"));//val_0  -》key值存在，直接返回oldValue
        System.out.println(map.computeIfAbsent(7,key -> "value_"+key));//value_7 -》key匹配不到，直接新增，返回值为value
        System.out.println(map.computeIfAbsent(8,key -> "88"));//88 -》key匹配到了，value为null，返回值作为value
        System.out.println(map.toString());//{0=val_0, 1=val_1, 2=val_2, 3=val_3, 4=val_4, 5=val_5, 7=value_7, 8=88, 10=700000}
        
    8. V computeIfPresent(K key,BiFunction remappingFunction)：
         * 根据key做匹配，如果匹配不上则返回null,匹配上根据BiFunction的apply方法获取value，返回value。
         * BiFunction接口的apply的入参为key、oldValue，调用computeIfPresent时重写Function接口
         * 可以根据key和oldValue进行逻辑处理，apply的返回值如果为null则删除该节点，否则即为要存储的value。
        System.out.println(map.toString());//{1=val_1, 2=val_2, 3=val_3, 4=val_4, 5=val_5, 10=null}
        System.out.println(map.computeIfPresent(3,(key,value) -> key+":"+value));//3:val_3 -》key存在，根据返回值修改value
        System.out.println(map.computeIfPresent(0,(key, value) -> "0000"));//null -》key不存在，返回null,不做任何操作
        System.out.println(map.computeIfPresent(1,(key, value) -> null));//null -》key存在，根据返回值修改value
        System.out.println(map.computeIfPresent(10,(key,value) -> "val_10"));//null -》oldValue值为null，删除节点
        System.out.println(map.toString());//{2=val_2, 3=3:val_3, 4=val_4, 5=val_5, 10=null}
        
        
##### 总结：
        1、流的速度比一般for循环慢，大概是几十倍，但是都是毫秒级别，总体来说影响不大，用一点效率换代码规范和可维护性是值得的。
        2、如果是有Integer类型等，一定注意装箱和拆箱，因为自动装箱和拆箱操作会大大降低性能，推荐使用mapToInt代替map。
        3、并行流并不总是比顺序流快，对于较小的数据量推荐用顺序流。
        4、对于一个函数里面有多个Stream流操作（例如多个遍历），第一个Stream（或者foreach）会比较慢，通常就是如1说的几十倍，但是后面几个流的函数运行效率会大大提高（未知原因，可能是第一次创建流需要大量时间，或者是预加载需要时间）。
        5、推荐多使用流操作ArrayList，而不是LinkedList。
        6、经常使用Java8会让代码变得优雅且好维护，但是也会使你的算法水平变得越来越低。
