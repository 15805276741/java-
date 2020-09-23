# java-
学校做的一些小项目

项目说明：在一定时间内，输入系统随机产生的字符串，正确慢慢升级，错误就退出系统

Play类


    package game2;

    import java.util.Scanner;

    public class Player {
   
       //玩家当前级别
   
	private int levelNo;
  
	//玩家当前得分
  
	private int perScore;
  
	//玩家的开始时间
  
	private long startTime;
  
	//玩家的已用时间
  
	private long elapsedTime;
  
	public void setLevelNo(int levelNo) {
		this.levelNo = levelNo;
	}
	public int getLevelNo(){
		return this.levelNo;
	}
	
	public void setPerScore(int perScore) {
		this.perScore = perScore;
	}
	public int getPerSocre(){
		return this.perScore;
	}
	public void setStartTime(long startTime) {
		this.startTime = startTime;
	}
	public long getStartTime() {
		return this.startTime ; 
	}
	public void setElapsedTime(long elapsedTime) {
		this.elapsedTime = elapsedTime;
	}
	public long getElapsedTime() {
		return this.elapsedTime ; 
	}
	@Override
	public String toString(){
		return "玩家的当前级别："+this.levelNo+",当前得分为："+this.perScore+",已用时间："+this.elapsedTime;
	}
	
	//玩
	public void play(){
		Scanner input =new Scanner(System.in);
		//玩家和电脑玩
		Game g = new Game();
		//将当前玩家传给电脑
		g.setPlayer(this);
		//外层循环：一共玩多少级。
		for(int i = 0 ;i<LevelParam.LEVEL_PARAMS.length;i++){
			//升级操作
			levelNo=levelNo+1;
			//返回当前时间距离1970年1-1之间的毫秒数
			startTime=System.currentTimeMillis();
			//分数的清零操作
			perScore=0;
			for(int j=0;j< LevelParam.LEVEL_PARAMS[i].getStrTimes();j++) {
				String out = g.GenStr();
				System.out.println("系统输出："+out);
				System.out.print("请输入：");
				String in = input.next();
				g.compareResult(out, in);
			}
		}
	   }
     }

Game类

     package game2;

     import java.util.Random;

        public class Game {
                private Player player;
                public void setPlayer(Player player) {
                        this.player = player;
                }
                public Player getPlayer(){
                        return this.player;
                }
                /**
                 * 系统产生的随机字符串：
                 *    玩家等级发生变化：字符串的长度会变长
                 * @return
                 */
                public String GenStr(){
                        Random r = new Random();
                        String message = "";
                        //获取当前级别
                        Level currentLevel = LevelParam.LEVEL_PARAMS[this.getPlayer().getLevelNo()-1];
                        for(int i = 0 ;i<currentLevel.getStrLength();i++){
                                int num = r.nextInt(currentLevel.getStrLength());
                                switch (num) {
                                        case 0:
                                                message = message +"a";
                                                break;
                                        case 1:
                                                message = message +"b";
                                                break;
                                        case 2:
                                                message = message +"c";
                                                break;
                                        case 3:
                                                message = message +"d";
                                                break;
                                        case 4:
                                                message = message +"e";
                                                break;
                                        case 5:
                                                message = message +"f";
                                                break;
                                        case 6:
                                                message = message +"g";
                                                break;
                                        case 7:
                                                message = message +"z";
                                                break;
                                }
                        }
                        return message ; 
                }
                /**
                 * 比较系统的输出和用户的输入
                 * @param out:系统的输出
                 * @param in：玩家的输入
                 */
                public void compareResult(String out,String in) {
                        if(out.equals(in)) {
                                //取得结束时间
                                long endTime = System.currentTimeMillis();
                                long diff = (endTime-this.getPlayer().getStartTime())/1000;
                                this.player.setElapsedTime(diff);
                                Level currentLevel = LevelParam.LEVEL_PARAMS[this.getPlayer().getLevelNo()-1];
                                if(diff<=currentLevel.getTimeLimit()) {
                                        this.player.setPerScore(this.player.getPerSocre()+currentLevel.getPerScore());
                                        System.out.println(this.player.toString());
                                        //通关判断。
                                        Level max = LevelParam.LEVEL_PARAMS[LevelParam.LEVEL_PARAMS.length-1];
                                        if(max.getLevelNo()==this.player.getLevelNo()) {
                                                if(this.player.getPerSocre()>=max.getPerScore()*max.getStrTimes()) {
                                                        System.out.println("恭喜你，通关了！！！");
                                                }
                                        }
                                }else{
                                        System.out.println("对不起,输入超时，系统退出！！！");
                                        //退出应用程序
                                        System.exit(-1);
                                }
                        }else{
                                System.out.println("对不起,输入错误，系统退出！！！");
                                //退出应用程序
                                System.exit(-1);
                        }
                }
        }


Level类

        package game2;
        /**
         * 等级类
         * @author Administrator
         */
        public class Level {
                //级别号
                private int levelNo;
                //字符串的长度
                private int strLength;
                //当前级别输入字符串的次数
                private int strTimes;
                //当前的级别限制时间
                private long timeLimit;
                //级别的每次得分
                private int perScore;

                public Level() {}
                public Level(int levelNo, int strLength, int strTimes, long timeLimit,
                                int perScore) {
                        this.levelNo = levelNo;
                        this.strLength = strLength;
                        this.strTimes = strTimes;
                        this.timeLimit = timeLimit;
                        this.perScore = perScore;
                }
                //alt+shift+s 
                public int getLevelNo() {
                        return levelNo;
                }
                public void setLevelNo(int levelNo) {
                        this.levelNo = levelNo;
                }
                public int getStrLength() {
                        return strLength;
                }
                public void setStrLength(int strLength) {
                        this.strLength = strLength;
                }
                public int getStrTimes() {
                        return strTimes;
                }
                public void setStrTimes(int strTimes) {
                        this.strTimes = strTimes;
                }
                public long getTimeLimit() {
                        return timeLimit;
                }
                public void setTimeLimit(long timeLimit) {
                        this.timeLimit = timeLimit;
                }
                public int getPerScore() {
                        return perScore;
                }
                public void setPerScore(int perScore) {
                        this.perScore = perScore;
                }
        }

LevelParam类

        package game2;
        public class LevelParam {
                public static final Level[] LEVEL_PARAMS= new Level[7];
                static{
                        LEVEL_PARAMS[0]= new Level(1,2,10,30,1);
                        LEVEL_PARAMS[1]= new Level(2,3,9,28,2);
                        LEVEL_PARAMS[2]= new Level(3,4,8,26,3);
                        LEVEL_PARAMS[3]= new Level(4,5,7,22,4);
                        LEVEL_PARAMS[4]= new Level(5,6,6,20,5);
                        LEVEL_PARAMS[5]= new Level(6,7,5,18,6);
                        LEVEL_PARAMS[6]= new Level(7,8,4,16,7);
                }
        }

启动类

        package game2;
        public class StartGame {
                public static void main(String[] args) {
                        new Player().play();
                }
        }

