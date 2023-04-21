#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct course
{
    char id[10];
    char name[20];
    char type[10];
    int total_hours;
    int lecture_hours;
    int lab_hours;
    double credits;
    char term[20];
    char time[20];
    struct course *next;
} Course;

// 读取写入数据到文件，实现了链表与文件的连接
void save_courses_to_file(Course *course_list)
{
    FILE *fp;
    fp = fopen("courses.txt", "w");
    Course *current_course = course_list;
    while (current_course != NULL)
    {
        fwrite(current_course, sizeof(Course), 1, fp); // 每次输入一个Course*size字节的数据到courses.txt里面
        current_course = current_course->next;
    }
    fclose(fp);
}
void save_courses_to_file_s(Course *course_list_s)
{
    FILE *fp;

    fp = fopen("courses_s.txt", "w");

    Course *current_course = course_list_s;
    while (current_course != NULL)
    {
        fwrite(current_course, sizeof(Course), 1, fp);
        current_course = current_course->next;
    }
    fclose(fp);
}
// 读取写入数据到文件
Course *load_courses_from_file()
{
    FILE *fp;
    fp = fopen("courses.txt", "r");
    if (fp == NULL)
    {
        return NULL;
    }
    Course *course_list = NULL;
    Course *last_course = NULL;
    Course *new_course;
    while (!feof(fp))
    {
        new_course = (Course *)malloc(sizeof(Course));
        if (fread(new_course, sizeof(Course), 1, fp) != 1) // 如果成功读取了一个Course型对象，则函数返回 1，否则返回其他值。
        {
            free(new_course);
            break;
        }
        if (course_list == NULL)
        {
            course_list = new_course;
        }
        else
        {
            last_course->next = new_course;
        }
        last_course = new_course;
    }
    fclose(fp);
    return course_list;
}
// 读取文件数据
Course *load_courses_from_file_s()
{
    FILE *fp;
    fp = fopen("courses_s.txt", "r");
    if (fp == NULL)
    {
        return NULL;
    }
    Course *course_list = NULL;
    Course *last_course = NULL;
    Course *new_course;
    while (!feof(fp))
    {
        new_course = (Course *)malloc(sizeof(Course));
        if (fread(new_course, sizeof(Course), 1, fp) != 1)
        {
            free(new_course);
            break;
        }
        if (course_list == NULL)
        {
            course_list = new_course;
        }
        else
        {
            last_course->next = new_course;
        }
        last_course = new_course;
    }
    fclose(fp);
    return course_list;
}

// 添加课程（管理员）
void add_course_M(Course *course_list)
{
    int w = 1, n, password;
    Course *new_course = (Course *)malloc(sizeof(Course));
    for (; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }
    printf("请输入管理员密码：\n__________\b\b\b\b\b\b\b\b\b\b"); // 设置密码
    scanf("%d", &password);
    if (password != 2022051068)
        return;
    for (; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }
    /*if (password != 2022051068)
      return;*/
    printf("课程编号\t课程名称\t课程性质\t总学时\t授课学时\t实验/上机学时\t学分\t开课学期\t时间\n");
    scanf("%s%s%s%d%d%d%lf%s%s", new_course->id, new_course->name, new_course->type,
          &(new_course->total_hours), &(new_course->lecture_hours),
          &(new_course->lab_hours), &(new_course->credits), new_course->term, new_course->time);
    new_course->next = NULL;
    if (course_list == NULL) // 头指针为空时
    {
        course_list = new_course;
    }
    else
    {
        Course *current_course = course_list;
        while (current_course->next != NULL)
        {
            current_course = current_course->next;
        }
        current_course->next = new_course;
    }
    save_courses_to_file(course_list);
    printf("输入1才能返回上一级菜单:[ ]\b\b");
    scanf("%d", &n);
    for (; n != 1;)
    {
        printf("输入1才能返回上一级菜单:[ ]\b\b");
        scanf("%d", &n);
    }
    return;
}
int check_for_plagiarism(Course *course_list_s, char id[20]) // 遍历链表巡查重复编号
{
    int i = 1;
    Course *current_s = course_list_s;
    while (current_s != NULL)
    {
        if (strcmp(current_s->id, id) == 0)
            return i;
        current_s = current_s->next;
    }
    return 0;
}
double check_for_allcredits(Course *course_list_s) // 检查已选课程总学分是否达到上限
{
    double sum = 0;
    Course *current_s = course_list_s;
    while (current_s != NULL)
    {
        sum += current_s->credits;
        if (current_s->next == NULL)
        {
            break;
        }
        current_s = current_s->next;
    }

    // 检查学分总和是否大于80，并相应地处理错误
    if (sum > 80)
    {
        printf("Error: Exceeded maximum amount of credits (80)\n");
        return 0;
    }
    return sum;
}
int check_repeat_time(Course *course_list_s, Course *new_course) // 检查当前要选课程时间是否与已选课程时间冲突
{
    Course *current_s = course_list_s;
    if (current_s == NULL)
        return 1;
    while (current_s->next != NULL)
    {
        if ((strcmp(current_s->time, new_course->time) == 0) && (strcmp(current_s->term, new_course->term) == 0)) // 检查学期和上课时间是否同时重复
        {
            return 0;
        } // 时间冲突 }
        current_s = current_s->next;
    }
    if ((strcmp(current_s->time, new_course->time) == 0) && (strcmp(current_s->term, new_course->term) == 0))
    {
        return 0;
    }
    return 1;
}

Course *add_course_s(Course *course_list, Course *course_list_s) // 学生添加课程操作
{
    int result, t;
    double sum = 0;
    char id[20], back;
    for (int w = 1; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }
    printf("请输入需要选择的课程编号：\n__________\b\b\b\b\b\b\b\b\b\b");

    scanf("%s", id);
    Course *current = course_list;
    Course *current_s = course_list_s;
    course *p;
    course *p1;
    p = (Course *)malloc(sizeof(Course));
    result = check_for_plagiarism(current_s, id);
    if (result != 0)
    {
        printf("添加失败已经选了此课程\n输入1才能返回上一级菜单:[ ]\b\b");
        scanf("%s", &back);
        return course_list_s;
    }

    sum = check_for_allcredits(current_s);
    if (sum > 80)
    {
        printf("添加失败达到课程总学分上限80分\n输入1才能返回上一级菜单:[ ]\b\b");
        scanf("%s", &back);
        return course_list_s;
    }

    while (current != NULL)
    {

        if (strcmp(current->id, id) == 0)
        {
            sum = sum + current->credits;
            if (sum > 80)
            {
                printf("添加失败达到课程总学分上限80分\n输入1才能返回上一级菜单:[ ]\b\b");
                scanf("%s", &back);
                return course_list_s;
            }
            t = check_repeat_time(current_s, current);
            if (t == 0)
            {
                printf("添加失败时间冲突\n输入1才能返回上一级菜单:[ ]\b\b");
                scanf("%s", &back);
                return course_list_s;
            }
            p->credits = current->credits;
            p->lab_hours = current->lab_hours;
            p->lecture_hours = current->lecture_hours;
            p->total_hours = current->total_hours;
            strcpy(p->id, current->id);
            strcpy(p->name, current->name);
            strcpy(p->type, current->type);
            strcpy(p->term, current->term);
            strcpy(p->time, current->time);
            p->next = NULL;
            if (current_s == NULL)
            {
                current_s = p;
                course_list_s = p;
            }
            else // 因为不能直接current_s->next = p;会造成p的后续连接，但我们只需要p所在节点的数据，则将p的数据赋予p1，将p1的next设为空再current_s->next = p1；
            {
                while (current_s->next != NULL)
                    current_s = current_s->next;
                p1 = (Course *)malloc(sizeof(Course));
                strcpy(p1->id, p->id);
                strcpy(p1->name, p->name);
                strcpy(p1->type, p->type);
                strcpy(p1->term, p->term);
                strcpy(p1->time, p->time);
                p1->credits = p->credits;
                p1->lab_hours = p->lab_hours;
                p1->lecture_hours = p->lecture_hours;
                p1->total_hours = p->total_hours;
                p1->next = NULL;
                current_s->next = p1;
            }
            /*while(check!=NULL)
              {
              printf("**%s", check->id);
              check=check->next;
              }*/
            // 检查链表被更改问题
            printf("添加成功\n输入1才能返回上一级菜单:[ ]\b\b");
            scanf("%s", &back);

            return course_list_s;
        }
        current = current->next;
    }
    printf("添加失败没有此课程\n输入1才能返回上一级菜单:[ ]\b\b");
    scanf("%s", &back);
    return course_list_s;
}
Course *delete_course_M(Course *course_list) // 总课程删减（管理员）
{
    int w = 1, password;
    char id[20];
    for (; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }

    printf("请输入管理员密码：\n__________\b\b\b\b\b\b\b\b\b\b");
    scanf("%d", &password);
    if (password != 2022051068)
        return course_list;
    for (; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }

    printf("请输入需要删除的课程编号：\n__________\b\b\b\b\b\b\b\b\b\b");
    scanf("%s", id);
    Course *prev = NULL;
    Course *current = course_list;
    while (current != NULL)
    {
        if (strcmp(current->id, id) == 0)
        {
            if (prev != NULL)
            {
                prev->next = current->next;
            }
            else
            {
                course_list = current->next; // 删除头节点
            }
            free(current);
            printf("删除成功！\n");
            return course_list;
        }
        prev = current;
        current = current->next;
    }
    printf("找不到该课程！\n");

    return course_list;
}

Course *delete_course_s(Course *course_list_s) // 删除已选课程（学生）
{
    int w = 1, password;
    char id[20];
    for (; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }
    /*
      printf("请输入管理员密码：\n__________\b\b\b\b\b\b\b\b\b\b");
      scanf("%d",&password);
      if(password!=2022051068)
      return course_list;*/
    for (; w == 1;)
    {
        system("cls"); // 终端清屏处理
        break;
    }

    printf("请输入需要删除的课程编号：\n__________\b\b\b\b\b\b\b\b\b\b");
    scanf("%s", id);
    Course *prev = NULL;
    Course *current = course_list_s;
    while (current != NULL)
    {
        if (strcmp(current->id, id) == 0)
        {
            if (prev != NULL)
            {
                prev->next = current->next;
            }
            else
            {
                course_list_s = current->next; // 删除头节点
            }
            free(current);
            printf("删除成功！\n");
            return course_list_s;
        }
        prev = current;
        current = current->next;
    }
    printf("找不到该课程！\n");

    return course_list_s;
}

void view_all_courses(Course *course_list) // 课程信息浏览（全部）
{
    int w = 1, n;
    if (w == 1)
        system("cls"); // 终端清屏处理
    printf("课程编号\t课程名称\t课程性质\t总学时\t授课学时\t实验/上机学时\t学分\t开课学期\t上课时间\n");
    Course *current_course = course_list;
    while (current_course != NULL)
    {
        printf("  %s\t\t%s\t%s\t\t %d\t%d\t\t%d\t\t%.1lf\t%s\t      %s \n",
               current_course->id, current_course->name,
               current_course->type, current_course->total_hours,
               current_course->lecture_hours, current_course->lab_hours,
               current_course->credits, current_course->term, current_course->time);
        current_course = current_course->next;
    }
    printf("输入1才能返回上一级菜单:[ ]\b\b");
    scanf("%d", &n);
    for (; n != 1;)
    {
        printf("输入1才能返回上一级菜单:[ ]\b\b");
        scanf("%d", &n);
    }
    return;
}

void view_all_courses_s(Course *course_list) // 已选课程浏览
{
    int w = 1, n;
    double sum = 0;
    if (w == 1)
        system("cls"); // 终端清屏处理
    printf("课程编号\t课程名称\t课程性质\t总学时\t授课学时\t实验/上机学时\t学分\t开课学期\t上课时间\n");
    Course *current_course = course_list;
    while (current_course != NULL)
    {
        printf("  %s\t\t%s\t%s\t\t %d\t%d\t\t%d\t\t%.1lf\t%s\t      %s \n",
               current_course->id, current_course->name,
               current_course->type, current_course->total_hours,
               current_course->lecture_hours, current_course->lab_hours,
               current_course->credits, current_course->term, current_course->time);
        sum = sum + current_course->credits;
        current_course = current_course->next;
    }
    printf("\n当前您选课的总学分为：%.1lf\n", sum);
    if (sum < 60)
        printf("当前您选课的总学分与最低选课总学分60还差：%.1lf分\n\n", 60.0 - sum);
    printf("输入1才能返回上一级菜单:[ ]\b\b");
    scanf("%d", &n);
    for (; n != 1;)
    {
        printf("输入1才能返回上一级菜单:[ ]\b\b");
        scanf("%d", &n);
    }
    return;
}

void search_course_by_credits(Course *course_list) // 课程查询（学分查询）
{
    int credits, n, w = 1;
    if (w == 1)
        system("cls"); // 终端清屏处理
    printf("输入所查询课程的学分:[ ]\b\b");
    scanf("%d", &credits);
    Course *current_course = course_list;
    while (current_course != NULL)
    {
        if (current_course->credits == credits)
        {
            printf("%s\t%s\t%s\t%d\t%d\t%d\t%.1lf\t%s\t%s\n",
                   current_course->id, current_course->name,
                   current_course->type, current_course->total_hours,
                   current_course->lecture_hours, current_course->lab_hours,
                   current_course->credits, current_course->term, current_course->time);
        }
        current_course = current_course->next;
    }
    for (; n != 1;)
    {
        printf("输入1才能返回上一级菜单:[ ]\b\b");
        scanf("%d", &n);
    }
}

int main()
{

    int choice, flag = 1;
    Course *course_list = load_courses_from_file();
    Course *course_list_s = load_courses_from_file_s();
    while (flag == 1)
    {
        system("cls"); // 终端清屏处理
        printf("\n\n\n\n\n\n\n\t\t\t\t╔═══════════════ MAIN MENU ═════════════════╗");
        printf("\n\t\t\t\t║ 1.添加总课程（管理员操作）                ║\n");
        printf("\t\t\t\t║ 2.修改总课程表                            ║\n");
        printf("\t\t\t\t║ 3.浏览总课程表                            ║\n");
        printf("\t\t\t\t║ 4.学生选课                                ║\n");
        printf("\t\t\t\t║ 5.修改学生课程表                          ║\n");
        printf("\t\t\t\t║ 6.查看学生课程表                          ║\n");
        printf("\t\t\t\t║ 7.按学分查询课程                          ║\n");
        printf("\t\t\t\t║ 8.退出系统                                ║");
        printf("\n\t\t\t\t╚═══════════════════════════════════════════╝\n\n");
        printf("\t\t\t\tEnter your choice: ");
        scanf("%d", &choice);
        switch (choice)
        {
        case 1:
            add_course_M(course_list);
            system("pause");
            break;

        case 2:
            course_list = delete_course_M(course_list);
            save_courses_to_file(course_list);
            system("pause");
            break;
        case 3:
            view_all_courses(course_list);
            system("pause");
            break;

        case 4:
            course_list_s = add_course_s(course_list, course_list_s);
            save_courses_to_file_s(course_list_s); // 将新增学生选课信息保存到文件
            system("pause");
            break;

        case 5:
            course_list_s = delete_course_s(course_list_s);
            save_courses_to_file_s(course_list_s); // 将新增学生选课信息保存到文件
            system("pause");
            break;

        case 6:
            view_all_courses_s(course_list_s);
            system("pause");
            break;

        case 7:
            search_course_by_credits(course_list);
            system("pause");
            break;

        case 8:
            flag = 0;
            break;

        default:
            printf("\n\t\tInvalid choice. Please try again.\n");
            system("pause");
            break;
        }
    }

    printf("\n\t\tThank you for using our system. Goodbye!\n\n");
}
