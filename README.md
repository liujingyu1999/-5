学生选课系统设计
======

实验目的
-------
分析学生选课系统<br>
使用GUI窗体及其组件设计窗体界面<br>
完成学生选课过程业务逻辑编程<br>
基于文件保存并读取数据<br>
处理异常<br>

实验要求
--------
一、系统角色分析及类设计<br>
例如：学校有“人员”，分为“教师”和“学生”，教师教授“课程”，学生选择课程。<br>
定义每种角色人员的属性，及其操作方法。<br>
属性示例： 人员（编号、姓名、性别……）<br>
教师（编号、姓名、性别、所授课程、……）<br>
   学生（编号、姓名、性别、所选课程、……）<br>
   课程（编号、课程名称、上课地点、时间、授课教师、……）<br>
以上属性仅为示例，同学们可以自行扩展。
掌握字符串String及其方法的使用。<br>
掌握异常处理结构<br>


核心代码
---------
1.课程<br>
```		    
        package top.hiai.dao;
/**
 * @author www.hiai.top
 */
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import top.hiai.model.Course;
import top.hiai.util.StringUtil;



public class CourseDao {
	public int courseAdd(Connection con,Course course)throws Exception{
		String sql="insert into t_course value(null,?,?,?,?,0)";
		PreparedStatement  pstmt=con.prepareStatement(sql);
		pstmt.setString(1,course.getCourseName());
		pstmt.setString(2, course.getCourseTime());
		pstmt.setString(3, course.getCourseTeacher());
		pstmt.setInt(4, course.getCapacity());
		return pstmt.executeUpdate();

	}
	public ResultSet courseList(Connection con,Course course) throws SQLException{
		StringBuffer sb=new StringBuffer("select * from t_course");
		if(StringUtil.isNotEmpty(course.getCourseName())){
			sb.append(" and courseName like '%"+course.getCourseName()+"%'");
		}
		if(StringUtil.isNotEmpty(course.getCourseTime())){
			sb.append(" and courseTime like '%"+course.getCourseTime()+"%'");
		}
		if(StringUtil.isNotEmpty(course.getCourseTeacher())){
			sb.append(" and courseTeacher like '%"+course.getCourseTeacher()+"%'");
		}
PreparedStatement pstmt=con.prepareStatement(sb.toString().replaceFirst("and", "where"));
return pstmt.executeQuery();
		}
	public int courseDelete(Connection con,String courseId)throws Exception{
		String sql="delete from t_course where courseId=?";
		PreparedStatement  pstmt=con.prepareStatement(sql);
		pstmt.setString(1, courseId);
		return pstmt.executeUpdate();

	}
	public int courseModify(Connection con,Course course)throws Exception{
		String sql="update t_course set courseName=?,courseTime=?,courseTeacher=?,capacity=? where courseId=? ";
		PreparedStatement pstmt=con.prepareStatement(sql);
		pstmt.setString(1, course.getCourseName());
		pstmt.setString(2, course.getCourseTime());
		pstmt.setString(3, course.getCourseTeacher());
		pstmt.setInt(4,course.getCapacity() );
		pstmt.setInt(5, course.getCourseId());
		return pstmt.executeUpdate();
		
	}
	public ResultSet UnderFullList(Connection con,Course course) throws SQLException{
		String sql="select * from t_course where capacity>numSelected";
		PreparedStatement pstmt=con.prepareStatement(sql);
		return pstmt.executeQuery();
	}
	}

 ```
2．用户登录<br>
```		
   public class Admin {
	private int adminId;
	private String password;
	
	public Admin() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Admin(int adminId, String password) {
		super();
		this.adminId = adminId;
		this.password = password;
	}
	public int getAdminId() {
		return adminId;
	}
	public void setAdminId(int adminId) {
		this.adminId = adminId;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
```
3．界面大小设计
```
public void actionPerformed(java.awt.event.ActionEvent evt) {
				jb_resetActionPerformed(evt);
			}
		});

		javax.swing.GroupLayout layout = new javax.swing.GroupLayout(
				getContentPane());
		getContentPane().setLayout(layout);
		layout
				.setHorizontalGroup(layout
						.createParallelGroup(
								javax.swing.GroupLayout.Alignment.LEADING)
						.addGroup(
								layout
										.createSequentialGroup()
										.addGap(41, 41, 41)
										.addGroup(
												layout
														.createParallelGroup(
																javax.swing.GroupLayout.Alignment.LEADING)
														.addGroup(
																layout
																		.createSequentialGroup()
																		.addComponent(
																				jLabel1)
																		.addPreferredGap(
																				javax.swing.LayoutStyle.ComponentPlacement.RELATED)
																		.addComponent(
																				courseNameTxt,
																				javax.swing.GroupLayout.PREFERRED_SIZE,
																				144,
																				javax.swing.GroupLayout.PREFERRED_SIZE)
																		.addGap(
																				60,
																				60,
																				60)
																		.addComponent(
																				jLabel2)
																		.addPreferredGap(
																				javax.swing.LayoutStyle.ComponentPlacement.RELATED)
																		.addComponent(
																				courseTimeTxt,
																				javax.swing.GroupLayout.PREFERRED_SIZE,
																				144,
																				javax.swing.GroupLayout.PREFERRED_SIZE))
```
实验结果
--------
![](https://github.com/liujingyu1999/-5/blob/master/1.jpg)
![](https://github.com/liujingyu1999/-5/blob/master/2.jpg)
![](https://github.com/liujingyu1999/-5/blob/master/3.jpg)
![](https://github.com/liujingyu1999/-5/blob/master/5.jpg)

实验过程
-----------
查询资料，整理方向，理清代码，编译代码，，调试代码。<br>

编程感想
------------
在这次实验中，学会了如何设计程序，运用相应代码设计登录界面，在设计中我深知自己掌握的知识还远远不够，掌握的一些理论知识应用到实践中去，总会出现这样或那样的问题，不是理论没有掌握好，而是光知道书本上的知识是远远不够的，一定要把理论知识和实践结合起来。把学到的知识应用到时间中去，多做多练，才可以把理论的精华发挥出来。知识不是知道，了解就好，一定要去应用它，发展它，让它在现实生活中得到充分的应用，从而解决一些问题，这才是学习的根本目的。而且知识又不是单一的，它是互相联系的，学科与学科之间都有着内在的联系。2通过这次设计，我学会了和别人配合工作，因为一个人所学的知识不可能面面俱到的，只有通过合作，发挥自己的优点，体现团队精神，才能使工作做得更为出色。通过这次设计，我学到了许多书本上学不到的知识，增强了自己的动手能力。即将毕业我十分珍惜这次锻炼的机会，我按部就班的完成了自己的设计任务，但由于自己的知识水平有限，仍然存在很多的不足之处，恳请老师多多指教！当今的社会是竞争的社会，而人才的竞争则是竞争的焦点，毕业设计对于我们即将离校的同学来说，是离校前很好的一次锻炼，使我们各方面的能力都有了很大的提高，为我们踏出校门，走上社会增强了能力与自信！<br>
