---
title: java课程设计源码
layout: page
---


{% highlight java %}
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import javax.swing.*;
import java.sql.*;
//主类//######################################################
public class my1
{
    public static void main(String[] args)  //主方法
    {
    	//设置统一字体
        Font font=new Font("宋体",Font.PLAIN,15);
        UIManager.put("Button.font",font);
        UIManager.put("Label.font",font);
        UIManager.put("TextField.font",font);
        dljm();//调用登录界面方法
    }
    public static  void dljm()
    {
        final String userName1 = "xietao";
        final String passwrod1 = "123456";
        final String userName2 = "daguang";
        final String passwrod2 = "654321";
        final JFrame jFrame = new JFrame("学生综合评价系统登录");
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();
        jFrame.setBounds(((int)dimension.getWidth() - 400) / 2, ((int)dimension.getHeight() - 300) / 2, 400, 200);
        jFrame.setResizable(false);
        jFrame.setLayout(null);
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JLabel label1 = new JLabel("用户");label1.setBounds(60, 20, 50, 30);jFrame.add(label1);
        JLabel label2 = new JLabel("密码");label2.setBounds(60, 70, 50, 30);jFrame.add(label2);
        final JTextField text1 = new JTextField();text1.setBounds(115, 20, 170, 30);jFrame.add(text1);
        final JPasswordField text2 = new JPasswordField();text2.setBounds(115, 70, 170, 30);jFrame.add(text2);
        JButton button = new JButton("登录");
        button.setBounds(115, 120, 170, 40);
        jFrame.add(button);
        button.addActionListener(new ActionListener()
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                if((userName1.equals(text1.getText()) && passwrod1.equals(text2.getText()))  || (userName2.equals(text1.getText()) && passwrod2.equals(text2.getText())))
                {
                    //JOptionPane.showMessageDialog(null, "登陆成功", "提示", JOptionPane.INFORMATION_MESSAGE);
                    zjm();//登陆成功调用主界面方法
                    jFrame.dispose();
                }
                else
                {
                    JOptionPane.showMessageDialog(null, "错误 , 检查账号和密码", "提示", JOptionPane.ERROR_MESSAGE);
                   // text1.setText("");
                    text2.setText("");
                }
            }
        });
       

        jFrame.setVisible(true);
    }
    //主界面方法
    public static  void zjm()
    {
        JFrame s=new JFrame("学生综合评价系统");
        s.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        s.setLayout(null);
        s.setResizable(false);
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();
        s.setBounds(((int)dimension.getWidth() - 400) / 2, ((int)dimension.getHeight() - 420) / 2, 400, 420);
        JButton button1 = new JButton("查看所有");button1.setBounds(115, 20, 170, 40);
        JButton button2 = new JButton("增加");button2.setBounds(115, 80, 170, 40);
        JButton button3 = new JButton("删除");button3.setBounds(115, 140, 170, 40);
        JButton button4 = new JButton("修改");button4.setBounds(115, 200, 170, 40);
        JButton button5 = new JButton("查找");button5.setBounds(115, 260, 170, 40);
        JButton button6 = new JButton("关于");button6.setBounds(115, 320, 170, 40);
        s.add(button1);s.add(button2);s.add(button3);s.add(button4);s.add(button5);s.add(button6);
        
        //s.add(myb);
        //不同按钮新建不同的功能类
        button1.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                new ckframe();
            }
        });
        button4.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                new xgframe();
            }
        });
        button2.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                new zjframe();
            }
        });
        button3.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                new scframe();
            }
        });
        button5.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                new czframe();
            }
        });
        button6.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
            	JOptionPane.showMessageDialog(null, "    计科（1）班\n    解涛\n    吴海光\n    2015-03-07", "---关于---", JOptionPane.PLAIN_MESSAGE);
            }
        });
        
        s.setVisible(true);
    }
    
}

//窗口类：查看所有#####################################################

class ckframe extends JFrame implements ActionListener//查看 - 类
{
    static TextArea ta1;
    JLabel l1;
    JButton b1,b2,b3,b4,b5,b6;//排序方式按钮
    ckframe()
    {
        super("查看所有");
        setLayout(null);
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();//使窗口位于屏幕中间
        this.setBounds(((int)dimension.getWidth() - 840) / 2, ((int)dimension.getHeight() - 550) / 2, 840, 550);
        
       //图形界面绘制
        ta1=new TextArea("",20,10,TextArea.SCROLLBARS_VERTICAL_ONLY);//只有竖滚动条的文本区
        ta1.setFont(new Font("宋体",Font.PLAIN,16));
        ta1.setFocusable(false);
        ta1.setBounds(5, 50, 820, 500-80);
        add(ta1);

        l1=new JLabel("选择排序方式：");l1.setBounds(5, 10, 120, 30);add(l1);
        b1=new JButton("学号");b1.setBounds(130,10,100,30);add(b1);
        b2=new JButton("道德成绩");b2.setBounds(240,10,100,30);add(b2);
        b3=new JButton("学习成绩");b3.setBounds(350,10,100,30);add(b3);
        b4=new JButton("运动成绩");b4.setBounds(460,10,100,30);add(b4);
        b5=new JButton("表现能力");b5.setBounds(570,10,100,30);add(b5);
        b6=new JButton("平均分数");b6.setBounds(680,10,100,30);add(b6);
        //监听按钮
        b2.addActionListener(this);b1.addActionListener(this);b3.addActionListener(this); b4.addActionListener(this);b5.addActionListener(this);b6.addActionListener(this);

        this.setVisible(true);
        Statement statement=sqljava.initsql();
        try
        {
            String sql = "select * from stu order by 学号";
            ResultSet rs = statement.executeQuery(sql);
            printsy(rs);
        }catch(SQLException e){;}
    }
    //事件响应，不同的按钮不同的排序
    public void actionPerformed(ActionEvent e)
    {
        Statement statement=sqljava.initsql();
        if(e.getSource()==b1)
        {
            try
            {
                String sql = "select * from stu order by 学号";
                ResultSet rs = statement.executeQuery(sql);
                printsy(rs);
            }catch(SQLException f){;}
        }
        if(e.getSource()==b2)
        {
            try
            {
                String sql = "select * from stu order by 道德品质 desc";
                ResultSet rs = statement.executeQuery(sql);
                printsy(rs);
            }catch(SQLException f){;}
        }
        if(e.getSource()==b3)
        {
            try
            {
                String sql = "select * from stu order by 学习能力 desc";
                ResultSet rs = statement.executeQuery(sql);
                printsy(rs);
            }catch(SQLException f){;}
        }
        if(e.getSource()==b4)
        {
            try
            {
                String sql = "select * from stu order by 运动健康 desc";
                ResultSet rs = statement.executeQuery(sql);
                printsy(rs);
            }catch(SQLException f){;}
        }
        if(e.getSource()==b5)
        {
            try
            {
                String sql = "select * from stu order by 表现能力 desc";
                ResultSet rs = statement.executeQuery(sql);
                printsy(rs);
            }catch(SQLException f){;}
        }
        if(e.getSource()==b6)
        {
            try
            {
                String sql = "select * from stu order by 学习能力+表现能力+运动健康+道德品质 desc";
                ResultSet rs = statement.executeQuery(sql);
                printsy(rs);
            } catch(SQLException f){;}
        }
    }
//方法:根据结果集输出列表和统计信息
    public static void printsy(ResultSet rs) throws SQLException
    {

        ta1.setText("学号\t\t姓名\t\t性别\t\t道德品质\t学习能力\t运动健康\t表现能力\t平均分数\n\n");
        while(rs.next())
        {
            ta1.append(rs.getString("学号")+"\t");
            ta1.append(rs.getString("姓名")+"\t\t");
            ta1.append(rs.getString("性别")+"\t\t");
            ta1.append("   "+rs.getString("道德品质")+"\t\t");
            ta1.append("   "+rs.getString("学习能力")+"\t\t");
            ta1.append("   "+rs.getString("运动健康")+"\t\t");
            ta1.append("   "+rs.getString("表现能力")+"\t\t");
            ta1.append("   "+(rs.getInt("道德品质")+rs.getInt("学习能力")+rs.getInt("运动健康")+rs.getInt("表现能力"))/4+"\t\t\n\n");
        }
        ta1.append("----------+++++++++++++++++++++++-------------\n");
        ta1.append("统计数据:\n\n");
        int b1=0,b2=0,b3=0,b4=0;
        rs.close();
        Statement statement=sqljava.initsql();
        try{
            int tmp;
            ResultSet rsm = statement.executeQuery("select * from stu");
            rsm.last();
            tmp=rsm.getRow();
            // 结果集
            rsm=statement.executeQuery("select * from stu where 道德品质 Between 90 and 100");
            rsm.last();
            b1=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 道德品质 Between 75 and 89");
            rsm.last();
            b2=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 道德品质 Between 60 and 74");
            rsm.last();
            b3=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 道德品质 Between 0 and 59");
            rsm.last();
            b4=rsm.getRow()*100/tmp;

        }
        catch(SQLException f)
        {
            ;
        }

        String tj=String.format("道德品质：优秀%d%%，良好%d%%，一般%d%%，不及格%d%%\n\n",b1,b2,b3,b4);
        ta1.append(tj);

        try{
            int tmp;
            ResultSet rsm = statement.executeQuery("select * from stu");
            rsm.last();
            tmp=rsm.getRow();
            // 结果集
            rsm=statement.executeQuery("select * from stu where 学习能力 Between 90 and 100");
            rsm.last();
            b1=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 学习能力 Between 75 and 89");
            rsm.last();
            b2=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 学习能力 Between 60 and 74");
            rsm.last();
            b3=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 学习能力 Between 0 and 59");
            rsm.last();
            b4=rsm.getRow()*100/tmp;

        }
        catch(SQLException f){;}
        tj=String.format("学习能力：优秀%d%%，良好%d%%，一般%d%%，不及格%d%%\n\n",b1,b2,b3,b4);
        ta1.append(tj);

        try{
            int tmp;
            ResultSet rsm = statement.executeQuery("select * from stu");
            rsm.last();
            tmp=rsm.getRow();

            rsm=statement.executeQuery("select * from stu where 运动健康 Between 90 and 100");
            rsm.last();
            b1=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 运动健康 Between 75 and 89");
            rsm.last();
            b2=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 运动健康 Between 60 and 74");
            rsm.last();
            b3=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 运动健康 Between 0 and 59");
            rsm.last();
            b4=rsm.getRow()*100/tmp;

        }
        catch(SQLException f){;}

        tj=String.format("运动健康：优秀%d%%，良好%d%%，一般%d%%，不及格%d%%\n\n",b1,b2,b3,b4);
        ta1.append(tj);
        try{
            int tmp;
            ResultSet rsm = statement.executeQuery("select * from stu");
            rsm.last();
            tmp=rsm.getRow();

            rsm=statement.executeQuery("select * from stu where 表现能力 Between 90 and 100");
            rsm.last();
            b1=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 表现能力 Between 75 and 89");
            rsm.last();
            b2=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 表现能力 Between 60 and 74");
            rsm.last();
            b3=rsm.getRow()*100/tmp;
            rsm=statement.executeQuery("select * from stu where 表现能力 Between 0 and 59");
            rsm.last();
            b4=rsm.getRow()*100/tmp;
        }
        catch(SQLException f){;}
        tj=String.format("表现能力：优秀%d%%，良好%d%%，一般%d%%，不及格%d%%\n\n",b1,b2,b3,b4);
        ta1.append(tj);
        ta1.setCaretPosition(1);//光标回到起始
    }
}

//窗口类：查找操作#3#################################################
class czframe extends JFrame implements ActionListener,ItemListener
{
    JLabel l1,l2,l3,l4,l5,l6,l7,l8;
    JTextField t1,t2,t3,t4,t5,t6,t7,t8;
    Choice cho;
    JButton b1;
    czframe()
    {
        super("---查找---");
        setLayout(null);
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();
        this.setBounds(((int)dimension.getWidth() - 500) / 2, ((int)dimension.getHeight() - 500) / 2, 550, 500);
        //图形界面绘制
        cho=new Choice();
        cho.add("根据学号");cho.add("根据姓名");
        cho.setBounds(290,30,80,30);
        add(cho);
        cho.addItemListener(this);
        b1=new JButton("查找");b1.setBounds(400, 30, 100, 30);add(b1);
        l1=new JLabel("学号");l1.setBounds(60, 30, 50, 30);add(l1);
        t1=new JTextField();t1.setBounds(110, 30, 170, 30);add(t1);
        l2=new JLabel("姓名");l2.setBounds(60, 80, 50, 30);add(l2);
        t2=new JTextField();t2.setBounds(110, 80, 170, 30);add(t2);
        l3=new JLabel("性别");l3.setBounds(60, 130, 50, 30);add(l3);
        t3=new JTextField();t3.setBounds(110, 130, 170, 30);add(t3);
        l4=new JLabel("道德品质");l4.setBounds(30, 180, 70, 30);add(l4);
        t4=new JTextField();t4.setBounds(110, 180, 170, 30);add(t4);
        l5=new JLabel("学习能力");l5.setBounds(30, 230, 70, 30);add(l5);
        t5=new JTextField();t5.setBounds(110, 230, 170, 30); add(t5);
        l6=new JLabel("运动健康");l6.setBounds(30, 280, 70, 30);add(l6);
        t6=new JTextField(); t6.setBounds(110, 280, 170, 30);add(t6);
        l7=new JLabel("表现能力"); l7.setBounds(30, 330, 70, 30);add(l7);
        t7=new JTextField();t7.setBounds(110, 330, 170, 30);add(t7);
        l8=new JLabel("平均成绩");l8.setBounds(30, 380, 70, 30);add(l8);
        t8=new JTextField();t8.setBounds(110, 380, 170, 30);add(t8);
        b1.addActionListener(this);
        this.setVisible(true);
    }

    public void actionPerformed(ActionEvent e)
    {
        Statement statement=sqljava.initsql();
        int flag_cz=0;//flag:是否查找成功
        int czxm=0;//根据什么查找
        // TODO Auto-generated method stub
        if(cho.getSelectedIndex()==0)czxm=0;else czxm=1;
        if(e.getSource()==b1)
        {
            String s;
            String j;
            String sql = "select * from stu";
            try
            {
                ResultSet rs = statement.executeQuery(sql);
                while(rs.next())
                {
                    if(czxm==0){s=t1.getText();j=rs.getString("学号");}else {j=rs.getString("姓名");s=t2.getText();}
                    if(j.equals(s))//如果找到学号或姓名
                    {
                        flag_cz=1;
                        //补全信息
                        t1.setText(rs.getString("学号"));
                        t2.setText(rs.getString("姓名"));
                        t3.setText(rs.getString("性别"));
                        t4.setText(rs.getString("道德品质"));
                        t5.setText(rs.getString("学习能力"));
                        t6.setText(rs.getString("运动健康"));
                        t7.setText(rs.getString("表现能力"));
                        t8.setText((rs.getInt("道德品质")+rs.getInt("学习能力")+rs.getInt("运动健康")+rs.getInt("表现能力"))/4+"");
                        break;
                    }
                }
                if(flag_cz==0)
                {
                    JOptionPane.showMessageDialog(null, "找不到这个学号或姓名！", "提示", JOptionPane.ERROR_MESSAGE);
                }
            }
            catch (SQLException e1)
            {
                // TODO Auto-generated catch block
                e1.printStackTrace();//打印异常到控制台
            }
        }

    }

	@Override
	public void itemStateChanged(ItemEvent arg0) {
		// TODO Auto-generated method stub
		
	}
}

//窗口类:删除操作#######################################################

class scframe extends JFrame implements ActionListener
{

    JLabel l1,l2,l3,l4,l5,l6,l7;
    JTextField t1,t2,t3,t4,t5,t6,t7;
    JButton b1,b2;
    scframe()
    {
        super("---删除信息---");
        setLayout(null);
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();
        this.setBounds(((int)dimension.getWidth() - 500) / 2, ((int)dimension.getHeight() - 500) / 2, 500, 500);
        b1=new JButton("查看信息");b1.setBounds(290, 30, 100, 30);add(b1);
        b2=new JButton("确定删除");b2.setBounds(70, 380, 100, 30);add(b2);
        l1=new JLabel("学号");l1.setBounds(60, 30, 50, 30);add(l1);
        t1=new JTextField();t1.setBounds(110, 30, 170, 30);add(t1);
        l2=new JLabel("姓名");l2.setBounds(60, 80, 50, 30);add(l2);
        t2=new JTextField();t2.setBounds(110, 80, 170, 30);add(t2);
        l3=new JLabel("性别");l3.setBounds(60, 130, 50, 30);add(l3);
        t3=new JTextField();t3.setBounds(110, 130, 170, 30);add(t3);
        l4=new JLabel("道德品质");l4.setBounds(30, 180, 70, 30);add(l4);
        t4=new JTextField();t4.setBounds(110, 180, 170, 30);add(t4);
        l5=new JLabel("学习能力");l5.setBounds(30, 230, 70, 30);add(l5);
        t5=new JTextField();t5.setBounds(110, 230, 170, 30);add(t5);
        l6=new JLabel("运动健康");l6.setBounds(30, 280, 70, 30);add(l6);
        t6=new JTextField();t6.setBounds(110, 280, 170, 30);add(t6);
        l7=new JLabel("表现能力");l7.setBounds(30, 330, 70, 30);add(l7);
        t7=new JTextField();t7.setBounds(110, 330, 170, 30);add(t7);
        b1.addActionListener(this);
        b2.addActionListener(this);
        this.setVisible(true);
    }
    public void actionPerformed(ActionEvent e)
    {
        Statement statement=sqljava.initsql();
        int flag_cz=0;
        if(e.getSource()==b1)
        {
            String s=t1.getText();
            String j;
            String sql = "select * from stu";
            try
            {
                ResultSet rs = statement.executeQuery(sql);
                while(rs.next())
                {
                    j=rs.getString("学号");
                    if(j.equals(s))
                    {
                        flag_cz=1;
                        t2.setText(rs.getString("姓名"));
                        t3.setText(rs.getString("性别"));
                        t4.setText(rs.getString("道德品质"));
                        t5.setText(rs.getString("学习能力"));
                        t6.setText(rs.getString("运动健康"));
                        t7.setText(rs.getString("表现能力"));
                        break;
                    }
                }
                if(flag_cz==0)JOptionPane.showMessageDialog(null, "找不到这个学号！", "提示", JOptionPane.ERROR_MESSAGE);
            }
            catch (SQLException e1){e1.printStackTrace();}
        }
        if(e.getSource()==b2)
        {
            //sql : update stu set 姓名=t2.gettext.....,...where 学号=t1.gettext
            String sql="";
            sql=String.format("delete from stu where 学号=%s",t1.getText());
            try{statement.execute(sql);flag_cz=1;}catch (SQLException e1){e1.printStackTrace();flag_cz=0;}
            if(flag_cz==1)JOptionPane.showMessageDialog(null, "删除成功！","提示", JOptionPane.INFORMATION_MESSAGE);
        }
    }
}

//Statement对象用于执行不带参数的简单SQL语句

//sql和java连接  初始化操作类######################################################
class sqljava
{
  public static Statement initsql()
  {
      // 驱动程序名
      String driver = "com.mysql.jdbc.Driver";
      // URL指向要访问的数据库名scutcs
      String url = "jdbc:mysql://127.0.0.1:3306/mys";
      // MySQL配置时的用户名
      String user = "root";
      // MySQL配置时的密码
      String password = "4296527sql";
      Statement statement = null;
      try
      {
          // 加载驱动程序
          Class.forName(driver);
          // 连续数据库
          Connection conn = DriverManager.getConnection(url, user, password);
          if(!conn.isClosed())
              System.out.println("Succeeded connecting to the Database!");
          // statement用来执行SQL语句
          statement = conn.createStatement();
          System.out.println("-----------------");
          return statement;
      }
      catch(ClassNotFoundException e)
      {
          System.out.println("Sorry,can`t find the Driver!");
          e.printStackTrace();
      }
      catch(SQLException e){e.printStackTrace();}
      catch(Exception e){e.printStackTrace();}
      return statement;
  }
}

//窗口类：修改操作###############################################

class xgframe extends JFrame implements ActionListener
{
    JLabel l1,l2,l3,l4,l5,l6,l7;
    JTextField t1,t2,t3,t4,t5,t6,t7;
    JButton b1,b2;
    xgframe()
    {
        super("---修改信息---");
        setLayout(null);
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();
        this.setBounds(((int)dimension.getWidth() - 500) / 2, ((int)dimension.getHeight() - 500) / 2, 500, 500);
        b1=new JButton("补全信息");b1.setBounds(290, 30, 100, 30);add(b1);
        b2=new JButton("确定更改");b2.setBounds(70, 380, 100, 30);add(b2);
        l1=new JLabel("学号");l1.setBounds(60, 30, 50, 30);add(l1);
        t1=new JTextField();t1.setBounds(110, 30, 170, 30);add(t1);
        l2=new JLabel("姓名");l2.setBounds(60, 80, 50, 30);add(l2);
        t2=new JTextField();t2.setBounds(110, 80, 170, 30);add(t2);
        l3=new JLabel("性别");l3.setBounds(60, 130, 50, 30);add(l3);
        t3=new JTextField();t3.setBounds(110, 130, 170, 30);add(t3);
        l4=new JLabel("道德品质");l4.setBounds(30, 180, 70, 30);add(l4);
        t4=new JTextField();t4.setBounds(110, 180, 170, 30);add(t4);
        l5=new JLabel("学习能力");l5.setBounds(30, 230, 70, 30);add(l5);
        t5=new JTextField();t5.setBounds(110, 230, 170, 30);add(t5);
        l6=new JLabel("运动健康");l6.setBounds(30, 280, 70, 30);add(l6);
        t6=new JTextField();t6.setBounds(110, 280, 170, 30);add(t6);
        l7=new JLabel("表现能力");l7.setBounds(30, 330, 70, 30);add(l7);
        t7=new JTextField();t7.setBounds(110, 330, 170, 30);add(t7);
        b1.addActionListener(this);
        b2.addActionListener(this);
        this.setVisible(true);
    }

    public void actionPerformed(ActionEvent e)
    {
        Statement statement=sqljava.initsql();
        int flag_cz=0;
        // TODO Auto-generated method stub
        if(e.getSource()==b1)
        {
            String s=t1.getText();
            String j;
            String sql = "select * from stu";
            try
            {
                ResultSet rs = statement.executeQuery(sql);
                while(rs.next())
                {
                    j=rs.getString("学号");
                    if(j.equals(s))
                    {
                        flag_cz=1; 
                        t2.setText(rs.getString("姓名"));
                        t3.setText(rs.getString("性别"));
                        t4.setText(rs.getString("道德品质"));
                        t5.setText(rs.getString("学习能力"));
                        t6.setText(rs.getString("运动健康"));
                        t7.setText(rs.getString("表现能力"));
                        break;
                    }
                } 
                if(flag_cz==0){JOptionPane.showMessageDialog(null, "找不到这个学号！", "提示", JOptionPane.ERROR_MESSAGE);}
            }
            catch (SQLException e1){e1.printStackTrace();}
        }
        if(e.getSource()==b2)
        {
            //sql : update stu set 姓名=t2.gettext.....,...where 学号=t1.gettext
            String sql="";
            //格式化字符串 用于执行sql
            sql=String.format("update stu set 姓名=\"%s\",性别=\"%s\",道德品质=\"%s\",学习能力=\"%s\",运动健康=\"%s\",表现能力=\"%s\" where 学号=%s", t2.getText(),t3.getText(),t4.getText(),t5.getText(),t6.getText(),t7.getText(),t1.getText());
            try{statement.execute(sql);}
            catch (SQLException e1){e1.printStackTrace();}
            if(flag_cz==1)JOptionPane.showMessageDialog(null, "修改成功！","提示", JOptionPane.INFORMATION_MESSAGE);
        }
    }
}

//窗口类：增加操作#############################################

class zjframe extends JFrame implements ActionListener
{

    JLabel l1,l2,l3,l4,l5,l6,l7;
    JTextField t1,t2,t3,t4,t5,t6,t7;
    JButton b2;
    zjframe()
    {
        super("---增加信息---");

        setLayout(null);
        Dimension dimension = Toolkit.getDefaultToolkit().getScreenSize();
        this.setBounds(((int)dimension.getWidth() - 500) / 2, ((int)dimension.getHeight() - 500) / 2, 500, 500);
        b2=new JButton("完成添加");b2.setBounds(70, 380, 100, 30);add(b2);
        l1=new JLabel("学号");l1.setBounds(60, 30, 50, 30);add(l1);
        t1=new JTextField();t1.setBounds(110, 30, 170, 30);add(t1);
        l2=new JLabel("姓名");l2.setBounds(60, 80, 50, 30);add(l2);
        t2=new JTextField();t2.setBounds(110, 80, 170, 30);add(t2);
        l3=new JLabel("性别");l3.setBounds(60, 130, 50, 30);add(l3);
        t3=new JTextField();t3.setBounds(110, 130, 170, 30);add(t3);
        l4=new JLabel("道德品质");l4.setBounds(30, 180, 70, 30);add(l4);
        t4=new JTextField();t4.setBounds(110, 180, 170, 30);add(t4);
        l5=new JLabel("学习能力");l5.setBounds(30, 230, 70, 30);add(l5);
        t5=new JTextField();t5.setBounds(110, 230, 170, 30);add(t5);
        l6=new JLabel("运动健康");l6.setBounds(30, 280, 70, 30);add(l6);
        t6=new JTextField();t6.setBounds(110, 280, 170, 30);add(t6);
        l7=new JLabel("表现能力");l7.setBounds(30, 330, 70, 30);add(l7);
        t7=new JTextField();t7.setBounds(110, 330, 170, 30);add(t7);
        b2.addActionListener(this);
        this.setVisible(true);
    }

    public void actionPerformed(ActionEvent e)
    {
        Statement statement=sqljava.initsql();
        int flag_cz=0;
        // TODO Auto-generated method stub
        if(e.getSource()==b2)
        {
            //sql : insert into stu(姓名，。。。) values（t2.gettext.....,...）
            String sql="";
            //格式化字符串用于执行sql
            sql=String.format("insert into stu(学号,姓名,性别,道德品质,学习能力,运动健康,表现能力) values(%s,\"%s\",\"%s\",%s,%s,%s,%s)",t1.getText(), t2.getText(),t3.getText(),t4.getText(),t5.getText(),t6.getText(),t7.getText());
            try
            {
                if(statement.execute(sql));
                flag_cz=1;
            }
            catch (SQLException e1)
            {
                // TODO Auto-generated catch block
                e1.printStackTrace();
                flag_cz=0;
            }
            if(flag_cz==1)
            {
                JOptionPane.showMessageDialog(null, "添加成功！","提示", JOptionPane.INFORMATION_MESSAGE);
            }
            else
            {
                JOptionPane.showMessageDialog(null, "添加失败！检查输入","提示", JOptionPane.ERROR_MESSAGE);
            }
        }
    }
}
{% endhighlight %}
