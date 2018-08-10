# testtest

/*----------------data Table GridView Css----------------------*/

.EU_TableScroll {
    /*max-height: 350px;*/
    width: 100%;
    overflow: auto;
    border: 1px solid #ccc;
}

.EU_DataTable {
    border-collapse: collapse;
    width: 100%;
}

    .EU_DataTable tr th {
        background-color: #3AC0F2;
        color: #fff;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 12px;
        padding: 10px 5px 10px 5px;
        border: 1px solid #cccccc;
        text-transform: capitalize;
        text-align: center;
    }

    .EU_DataTable tr:nth-child(2n+2) {
        background-color: #f3f4f5;
    }

    .EU_DataTable tr:nth-child(2n+1) td {
        background-color: #dadbdc;
        color: #454545;
    }

    .EU_DataTable tr td {
        padding: 5px 10px 5px 10px;
        color: #454545;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 12px;
        border: 1px solid #cccccc;
        vertical-align: middle;
    }

        .EU_DataTable tr td:first-child {
            text-align: center;
        }


/*----------------PRB Design----------------------*/

  <asp:Panel ID="pnlViewTeamPRB" runat="server">
                    <div class="EU_TableScroll">
                        <asp:GridView ID="gridViewTeamPRB" runat="server" EnableViewState="false" AutoGenerateColumns="false"
                            CssClass="EU_DataTable" OnDataBound="gridViewTeamPRB_DataBound">
                            <Columns>
                                <asp:TemplateField HeaderText="Sno">
                                    <ItemTemplate>
                                        <%#Container.DataItemIndex+1 %>
                                    </ItemTemplate>
                                </asp:TemplateField>
                                <asp:BoundField DataField="EmpID" HeaderText="EmpID" />
                                <asp:BoundField DataField="TeamID" HeaderText="TeamID" />
                                <asp:BoundField DataField="TeamName" HeaderText="Team Name" />
                                <asp:BoundField DataField="EmpName" HeaderText="Emp Name" />
                                <asp:BoundField DataField="January" HeaderText="Jan" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="February" HeaderText="Feb" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="March" HeaderText="Mar" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="April" HeaderText="Apr" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="May" HeaderText="May" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="June" HeaderText="June" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="July" HeaderText="July" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="August" HeaderText="Aug" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="September" HeaderText="Sept" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="October" HeaderText="Oct" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="November" HeaderText="Nov" ItemStyle-HorizontalAlign="Center" />
                                <asp:BoundField DataField="December" HeaderText="Dec" ItemStyle-HorizontalAlign="Center" />
                            </Columns>
                            <EmptyDataTemplate>
                                <div class="errot">
                                    <span>No Record Found.</span>
                                </div>
                            </EmptyDataTemplate>
                        </asp:GridView>
                    </div>
                </asp:Panel>
                <asp:Panel ID="pnlSetTeamPRB" runat="server">
                    <div class="row">
                        <div class="contact-left clearfix">
                            <div class="select-dropdown">
                                <span class="focus-input100"></span>
                                <span class="symbol-input100">
                                    <i class="fa fa-user" aria-hidden="true">&nbsp;Employee</i>
                                </span>
                                <asp:DropDownList ID="ddlEmpList" runat="server" Style="display: block;">
                                </asp:DropDownList>
                            </div>
                            <div class="select-dropdown">
                                <span class="focus-input100"></span>
                                <span class="symbol-input100">
                                    <i class="fa fa-calendar" aria-hidden="true">&nbsp;Month</i>
                                </span>
                                <asp:DropDownList ID="ddlMonth" runat="server" Style="display: block;">
                                </asp:DropDownList>
                            </div>
                            <div class="select-dropdown">
                                <span class="focus-input100"></span>
                                <span class="symbol-input100">
                                    <i class="fa fa-calendar" aria-hidden="true">&nbsp;Year</i>
                                </span>
                                <asp:DropDownList ID="ddlYear" runat="server" Style="display: block;">
                                </asp:DropDownList>
                            </div>

                            <label>&nbsp;</label>
                            <label>1 - Poor&nbsp;&nbsp;&nbsp;2 – Fair&nbsp;&nbsp;&nbsp;3 – Average&nbsp;&nbsp;&nbsp;4 – Good&nbsp;&nbsp;&nbsp;5 – Excellent</label>
                            <div class="EU_TableScroll">
                                <asp:GridView ID="gridViewCriteria" runat="server" EnableViewState="false"
                                    AutoGenerateColumns="false" CssClass="EU_DataTable">
                                    <Columns>
                                        <asp:TemplateField HeaderText="Sno">
                                            <ItemTemplate>
                                                <%#Container.DataItemIndex+1 %>
                                            </ItemTemplate>
                                        </asp:TemplateField>

                                        <asp:BoundField DataField="CriteriaID" HeaderText="CriteriaID" Visible="false" />
                                        <asp:BoundField DataField="CriteriaName" HeaderText="Criteria" />
                                        <asp:TemplateField HeaderText="Rating">
                                            <ItemTemplate>
                                                <asp:RadioButtonList ID="rdoRating" runat="server" RepeatDirection="Horizontal">
                                                    <asp:ListItem Value="5" Text="Excellent"></asp:ListItem>
                                                    <asp:ListItem Value="4" Text="Good"></asp:ListItem>
                                                    <asp:ListItem Value="3" Text="Average"></asp:ListItem>
                                                    <asp:ListItem Value="2" Text="Fair"></asp:ListItem>
                                                    <asp:ListItem Value="1" Text="Poor"></asp:ListItem>
                                                </asp:RadioButtonList>
                                            </ItemTemplate>
                                        </asp:TemplateField>
                                        <asp:TemplateField HeaderText="Feedback">
                                            <ItemTemplate>
                                                <asp:TextBox ID="txtFeedback" runat="server"></asp:TextBox>
                                            </ItemTemplate>
                                        </asp:TemplateField>

                                    </Columns>
                                    <EmptyDataTemplate>
                                        <div class="errot">
                                            <span>No Record Found.</span>
                                        </div>
                                    </EmptyDataTemplate>
                                </asp:GridView>
                            </div>

                            <div class="input-field">
                                <asp:TextBox ID="txtOverAllPRB" runat="server" class="require"></asp:TextBox>
                                <label>
                                    Over All PRB</label>
                            </div>
                            <div class="input-field">
                                <asp:TextBox ID="txtComment" runat="server" TextMode="MultiLine" class="materialize-textarea"></asp:TextBox>
                                <textarea rows="7" name="message"></textarea>
                                <label>
                                    Comment</label>
                                <div class="response">
                                </div>
                            </div>
                            <%--<asp:Button ID="btnSave" runat="server" class="submitForm custom-btn waves-effect" />--%>

                            <button type="button" class="submitForm custom-btn waves-effect">
                                <i class="fa fa-floppy-o" aria-hidden="true"></i>Save</button>
                        </div>
                        <!-- /.contact-right -->
                    </div>
                </asp:Panel>
                
                
/*----------------PRB Code----------------------*/

using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.HtmlControls;
using System.Web.UI.WebControls;
using System.Data;
using System.Reflection;
using System.Text;
using System.Globalization;

public partial class EmpPRB : System.Web.UI.Page
{
    DataClassesDataContext objmain = new DataClassesDataContext();
    protected override void OnInit(EventArgs e)
    {
        base.OnInit(e);
        ViewStateUserKey = Session.SessionID;
    }

    string PageName;
    protected void Page_Load(object sender, EventArgs e)
    {
        Response.Cache.SetCacheability(HttpCacheability.NoCache);
        PageName = "EmpPRB";
        if (ViewStateUserKey.ToString() != Session.SessionID.ToString())
        {
            Session.Abandon();
            Response.Redirect("LogoutPage.aspx");
        }
        if (Session["EmpID"] == null || Session["UserName"] == null || Session["Role"] == null || Session["LoginActiveStatus"] == null)
        {
            Session.Abandon();
            Response.Redirect("LogoutPage.aspx");
        }

        Page.Header.Title = "HR Connect - PRB Details";

        if (!IsPostBack)
        {
            FillEmpPRBDetails();
            pnlSelfPRB.Visible = true;
            pnlViewTeamPRB.Visible = false;
            pnlSetTeamPRB.Visible = false;
            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "Page Load", "Page Load Successfully", Request.UserHostAddress, DateTime.Now);
        }

        switch ((int)Session["Role"])
        {
            case 1:
                btnSelfPRB.Visible = true;
                btnViewTeamPRB.Visible = true;
                btnSetTeamPRB.Visible = false;
                break;
            case 2:
                btnSelfPRB.Visible = true;
                btnViewTeamPRB.Visible = true;
                btnSetTeamPRB.Visible = true;
                break;
            case 3:
            default:
                btnSelfPRB.Visible = true;
                btnViewTeamPRB.Visible = false;
                btnSetTeamPRB.Visible = false;
                break;
        }
    }

    protected void FillEmpPRBDetails()
    {
        try
        {
            int CurrentYear = DateTime.Now.Year;

            var query = objmain.GetEmployeePRB((int)Session["EmpID"], (int)Session["Role"], DateTime.Now.Year, "Self").ToList();

            DataTable dt = LINQToDataTable(query);

            StringBuilder stringBuilder = new StringBuilder();

            foreach (DataRow row in dt.Rows)
            {
                int valuePRB = (int)row["PRB"];
                int percentagePRB = valuePRB * 100 / 20;
                int monthNumber = Convert.ToInt16(row["PRBMonth"]);
                string lineColor = string.Empty;
                string duration = string.Empty;
                string delay = string.Empty;

                switch (monthNumber)
                {
                    case 1:
                    case 7:
                        lineColor = "photoshop";
                        duration = "0.5s";
                        delay = ".5s";
                        break;

                    case 2:
                    case 8:
                        lineColor = "jquery";
                        duration = "1s";
                        delay = ".15s";
                        break;

                    case 3:
                    case 9:
                        lineColor = "php";
                        duration = "0.5s";
                        delay = ".25s";
                        break;

                    case 4:
                    case 10:
                        lineColor = "html5";
                        duration = "0.5s";
                        delay = ".5s";
                        break;

                    case 5:
                    case 11:
                        lineColor = "css3";
                        duration = "1s";
                        delay = ".15s";
                        break;

                    case 6:
                    case 12:
                        lineColor = "marketing";
                        duration = "1.5s";
                        delay = ".25s";
                        break;
                }

                stringBuilder.Append(@"<div class='single_experties'>");
                stringBuilder.Append(@"<div class='skilled-tittle'>").Append(GetMonthName(monthNumber));
                stringBuilder.Append("</div>");
                stringBuilder.Append(@"<div class='progress'>");
                stringBuilder.Append(@"<div class='progress-bar " + lineColor + @"-bg wow Rx-width-" + percentagePRB + @" animated' role='progressbar' data-wow-duration='" + duration + @"' data-wow-delay='" + delay + @"' aria-valuenow='0' aria-valuemin='0' aria-valuemax='100' > ");
                stringBuilder.Append(@"<span class='marketing-color'>").Append(valuePRB);
                stringBuilder.Append("</span>");
                stringBuilder.Append("</div>");
                stringBuilder.Append("</div>");
                stringBuilder.Append("</div>");
            }

            divSelfPRB.InnerHtml = stringBuilder.ToString();

            Chart1.Titles.Add("PRB Detail");
            Chart1.Series["Series1"].XValueMember = "PRBMonth";
            Chart1.Series["Series1"].YValueMembers = "PRB";
            Chart1.DataSource = query;
            Chart1.DataBind();

            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "View Employee Profile successfully", "View Employee Profile successfully", Request.UserHostAddress, DateTime.Now);

        }
        catch (Exception Ex)
        {
            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "View Employee Profile unsuccessfull", "View Employee Profile unsuccessfull", Request.UserHostAddress, DateTime.Now);
        }
    }

    protected void FillTeamPRBDetails()
    {
        try
        {
            int CurrentYear = DateTime.Now.Year;

            var query = objmain.GetEmployeePRBMonthName((int)Session["EmpID"], (int)Session["Role"], CurrentYear, "Team").ToList();

            gridViewTeamPRB.Columns[1].Visible = false; // Remove Emp ID
            gridViewTeamPRB.Columns[2].Visible = false; // Remove Team ID

            gridViewTeamPRB.DataSource = query;

            gridViewTeamPRB.DataBind();
            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "View Employee Profile successfully", "View Employee Profile successfully", Request.UserHostAddress, DateTime.Now);

        }
        catch (Exception Ex)
        {
            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "View Employee Profile unsuccessfull", "View Employee Profile unsuccessfull", Request.UserHostAddress, DateTime.Now);
        }
    }


    protected void gridViewTeamPRB_DataBound(object sender, EventArgs e)
    {
        for (int i = gridViewTeamPRB.Rows.Count - 1; i > 0; i--)
        {
            GridViewRow row = gridViewTeamPRB.Rows[i];
            GridViewRow previousRow = gridViewTeamPRB.Rows[i - 1];


            if (row.Cells[3].Text == previousRow.Cells[3].Text)
            {
                if (previousRow.Cells[3].RowSpan == 0)
                {
                    if (row.Cells[3].RowSpan == 0)
                    {
                        previousRow.Cells[3].RowSpan += 2;
                    }
                    else
                    {
                        previousRow.Cells[3].RowSpan = row.Cells[3].RowSpan + 1;
                    }
                    row.Cells[3].Visible = false;
                }
            }
        }

    }

    private DataTable LINQToDataTable<T>(IEnumerable<T> varlist)
    {
        DataTable dtReturn = new DataTable();

        // column names 
        PropertyInfo[] oProps = null;

        if (varlist == null) return dtReturn;

        foreach (T rec in varlist)
        {
            // Use reflection to get property names, to create table, Only first time, others will follow
            if (oProps == null)
            {
                oProps = ((Type)rec.GetType()).GetProperties();
                foreach (PropertyInfo pi in oProps)
                {
                    Type colType = pi.PropertyType;

                    if ((colType.IsGenericType) && (colType.GetGenericTypeDefinition()
                    == typeof(Nullable<>)))
                    {
                        colType = colType.GetGenericArguments()[0];
                    }

                    dtReturn.Columns.Add(new DataColumn(pi.Name, colType));
                }
            }

            DataRow dr = dtReturn.NewRow();

            foreach (PropertyInfo pi in oProps)
            {
                dr[pi.Name] = pi.GetValue(rec, null) == null ? DBNull.Value : pi.GetValue
                (rec, null);
            }

            dtReturn.Rows.Add(dr);
        }
        return dtReturn;
    }

    private string GetMonthName(int monthNumber)
    {
        string monthName = string.Empty;
        switch (monthNumber)
        {
            case 1:
                monthName = "January";
                break;
            case 2:
                monthName = "February";
                break;
            case 3:
                monthName = "March";
                break;
            case 4:
                monthName = "April";
                break;
            case 5:
                monthName = "May";
                break;
            case 6:
                monthName = "June";
                break;
            case 7:
                monthName = "July";
                break;
            case 8:
                monthName = "August";
                break;
            case 9:
                monthName = "September";
                break;
            case 10:
                monthName = "October";
                break;
            case 11:
                monthName = "November";
                break;
            case 12:
                monthName = "December";
                break;
        }
        return monthName;
    }

    protected void btnSelfPRB_Click(object sender, EventArgs e)
    {
        FillEmpPRBDetails();
        pnlSelfPRB.Visible = true;
        pnlViewTeamPRB.Visible = false;
        pnlSetTeamPRB.Visible = false;

        btnSelfPRB.Attributes.Add("class", "button waves-effect default is-checked");
        btnViewTeamPRB.Attributes.Add("class", "button waves-effect default");
        btnSetTeamPRB.Attributes.Add("class", "button waves-effect default");
    }

    protected void btnViewTeamPRB_Click(object sender, EventArgs e)
    {
        FillTeamPRBDetails();
        pnlSelfPRB.Visible = false;
        pnlViewTeamPRB.Visible = true;
        pnlSetTeamPRB.Visible = false;

        btnSelfPRB.Attributes.Add("class", "button waves-effect default");
        btnViewTeamPRB.Attributes.Add("class", "button waves-effect default is-checked");
        btnSetTeamPRB.Attributes.Add("class", "button waves-effect default");
    }

    protected void btnSetTeamPRB_Click(object sender, EventArgs e)
    {
        FillEmployeeList();
        FillMonthList();
        FillYearList();
        FillCriteriaList();
        pnlSelfPRB.Visible = false;
        pnlViewTeamPRB.Visible = false;
        pnlSetTeamPRB.Visible = true;

        btnSelfPRB.Attributes.Add("class", "button waves-effect default");
        btnViewTeamPRB.Attributes.Add("class", "button waves-effect default");
        btnSetTeamPRB.Attributes.Add("class", "button waves-effect default is-checked");
    }

    private void FillEmployeeList()
    {
        var query1 = objmain.TeamEmpDetails.Where(x=> x.EmpID == (int)Session["EmpID"]).Select(x => new { x.EmpID }).ToList();

    }

    private void FillMonthList()
    {
        ddlMonth.Items.Clear();
        ddlMonth.Items.Insert(0, new ListItem("--Select Month --", "00"));
        var months = CultureInfo.CurrentCulture.DateTimeFormat.MonthNames;
        for (int i = 0; i < months.Length - 1; i++)
        {
            ddlMonth.Items.Add(new ListItem(months[i], (i + 1).ToString()));
        }
    }

    private void FillYearList()
    {
        ddlYear.Items.Clear();
        ddlYear.Items.Add(new ListItem(DateTime.Now.Year.ToString(), DateTime.Now.Year.ToString()));
    }

    private void FillCriteriaList()
    {
        try
        {
            var query = objmain.CriteriaMasters.Where(x => x.IsActive == 'A').Select(x => new { x.CriteriaID, x.CriteriaName });
            if (query != null && query.Count() != 0)
            {
                gridViewCriteria.DataSource = query;
                gridViewCriteria.DataBind();
            }
        }
        catch (Exception Ex)
        { }
    }

}
