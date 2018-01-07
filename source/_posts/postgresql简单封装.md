---
title: PostgreSQL简单封装
id: 112
categories:
  - C++
  - 系统编程
date: 2011-03-01 00:26:00
tags:
---

    

工作需要，虽然有libpqxx，但是目前为了刚刚够用，尽量不要引入第三方库。在网上找到简单的封装[http://www.touspassagers.com/2010/12/a-postgresql-database-interface-wrapper-in-c/](http://www.touspassagers.com/2010/12/a-postgresql-database-interface-wrapper-in-c/)

&nbsp;

直接贴上代码:

<pre style="border: 1px dotted #785;background: #f5f5f5;">#include &lt;iostream&gt;
#include &lt;stdio.h&gt;
#include &lt;string&gt;
#include &lt;stdlib.h&gt;
#include &lt;libpq-fe.h&gt;
#include &lt;map&gt;
#include &lt;math.h&gt;
#include &lt;string.h&gt;
using namespace std;
typedef map&lt;int,map&lt;string,string&gt; &gt; map_result;
static void
finish_connection(PGconn *conn)
{
    PQfinish(conn);
}
class Conn
{
public:
    // Methods
    Conn(char *connstring);
    ~Conn();
    map_result fetch(char *SQL);
    int insert(char *SQL);
    void reset();
private:
    // Members
    PGconn *conn;
    const char *conninfo;
    PGresult *res;
};
Conn::Conn(char *connstring)
{
    // Connect to the DB
    conninfo = connstring;
    conn = PQconnectdb(conninfo);
    // See if the connection took:
    if (PQstatus(conn) != CONNECTION_OK)
    {
        fprintf(stderr, "Could not connect to db/n%s",
                PQerrorMessage(conn));
        finish_connection(conn);
    }
}
Conn::~Conn()
{
    finish_connection(conn);
}
void Conn::reset()
{
    PQfinish(conn);
    conn = PQconnectdb(conninfo);
}
map_result Conn::fetch(char *SQL)
{
    int row, col;
    map_result results;
    map&lt;string,string&gt; pairs;
    // Check the connection:
    if (PQstatus(conn) != CONNECTION_OK)
        this-&gt;reset();
    // Start a transaction:
    res = PQexec(conn, "BEGIN");
    if (PQresultStatus(res) != PGRES_COMMAND_OK)
    {
        fprintf(stderr, "Failed to BEGIN transaction /n%s",
                PQerrorMessage(conn));
        PQclear(res);
        finish_connection(conn);
    }
    // Set up fetch query with a CURSOR:
    string FinalSQL = string("DECLARE myportal CURSOR FOR ") +
                      string(SQL);
    // Declare CURSOR and execute query:
    res = PQexec(conn,FinalSQL.c_str());
    if (PQresultStatus(res) != PGRES_COMMAND_OK)
    {
        fprintf(stderr, "QUERY FAILED/n%s/n",
                PQerrorMessage(conn));
        PQclear(res);
        finish_connection(conn);
    }
    PQclear(res);
    // Fetch results:
    res = PQexec(conn, "FETCH ALL in myportal");
    if (PQresultStatus(res) != PGRES_TUPLES_OK)
    {
        fprintf(stderr, "FETCH ALL failed/n%s/n",
                PQerrorMessage(conn));
        PQclear(res);
        finish_connection(conn);
    }
    else
    {
        row = col = 0;
        for (row=0; row&lt;PQntuples(res); row++)
        {
            for(col=0; col&lt;PQnfields(res); col++)
            {
                pairs[PQfname(res,col)] = PQgetvalue(res, row, col);
            }
            results[row] = pairs;
        }
    }
    return results;
}
int Conn::insert(char *sql)
{
    // Check the connection:
    if (PQstatus(conn) != CONNECTION_OK)
        this-&gt;reset();
    // Start a transaction:
    res = PQexec(conn, "START TRANSACTION;");
    if (PQresultStatus(res) != PGRES_COMMAND_OK)
    {
        fprintf(stderr, "Failed to BEGIN transaction /n%s/n",
                PQerrorMessage(conn));
        PQclear(res);
        finish_connection(conn);
    }
    // Execute Insert:
    res = PQexec(conn, sql);
    if (PQresultStatus(res) != PGRES_COMMAND_OK)
    {
        fprintf(stderr, "Failed to execute INSERT /n%s/n",
                PQerrorMessage(conn));
        PQclear(res);
        finish_connection(conn);
    }
    // COMMIT transaction:
    res = PQexec(conn, "COMMIT");
    if (PQresultStatus(res) != PGRES_COMMAND_OK)
    {
        fprintf(stderr, "Failed to COMMIT transaction/n%s/n",
                PQerrorMessage(conn));
        PQclear(res);
        finish_connection(conn);
    }
    return 0;
}</pre>&nbsp;

使用代码：

<pre style="border: 1px dotted #785;background: #f5f5f5;">#include &lt;iostream&gt;
#include &lt;boost/lexical_cast.hpp&gt;
#include "PGConn.hpp"
using namespace std;

int main()
{
    map_result res;
    Conn *postgres = new Conn("dbname=mydb user=postgres");
    for( int i=0; i&lt;10; ++i )
    {
        std::string strTemp( "(" +boost::lexical_cast&lt;std::string&gt;(i) + "," );
        strTemp += boost::lexical_cast&lt;std::string&gt;(i+1) + ",";
        strTemp += boost::lexical_cast&lt;std::string&gt;(i+2) + ");";
        std::string strSql("INSERT INTO exam_en(language,math,computer) VALUES " + strTemp );
        postgres-&gt;insert( (char*)strSql.c_str() );
    }
    char sql[] = "SELECT * FROM exam_en;";
    res = postgres-&gt;fetch(sql);
    map_result::iterator it = res.begin();
    for( ; it!=res.end(); ++it )
    {
        std::cout &lt;&lt; "ROW:" &lt;&lt; it-&gt;first &lt;&lt; std::endl;
        map&lt;string,string&gt;::iterator itField = it-&gt;second.begin();
        for( ; itField!=it-&gt;second.end(); ++itField )
        {
            std::cout &lt;&lt; "Field:" &lt;&lt; itField-&gt;first &lt;&lt; "  Value:" &lt;&lt; itField-&gt;second &lt;&lt; std::endl;
        }
    }
    return 0;
}
</pre>&nbsp;

</div>