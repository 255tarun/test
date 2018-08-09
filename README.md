# testtest


public int TeamID { get; set; }

    public string TeamName { get; set; }

    public string EmpName { get; set; }

    public int January { get; set; }

    public int February { get; set; }

    public int March { get; set; }

    public int April { get; set; }

    public int May { get; set; }

    public int June { get; set; }

    public int July { get; set; }

    public int August { get; set; }

    public int September { get; set; }

    public int October { get; set; }

    public int November { get; set; }

    public int December { get; set; }
    
    
    
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
            int rowNumber = 1;

            foreach (DataRow row in dt.Rows)
            {
                int valuePRB = (int)row["PRB"];
                int percentagePRB = (int)(Convert.ToDecimal(valuePRB) / Convert.ToDecimal(.2));
                int monthNumber = Convert.ToInt16(row["PRBMonth"]);

                if (rowNumber == 1 || rowNumber == 4 || rowNumber == 7 || rowNumber == 10)
                {
                    stringBuilder.Append(@"<div class=""col l6 m6 s12"">");
                }

                switch (monthNumber)
                {
                    case 1:
                    case 7:
                        stringBuilder.Append(@"<div class=""single_experties"">");
                        stringBuilder.Append(@"<div class=""skilled-tittle"">").Append(GetMonthName(monthNumber));
                        stringBuilder.Append("</div>");
                        stringBuilder.Append(@"<div class=""progress"">");
                        stringBuilder.Append(@"<div class=""progress-bar photoshop-bg wow Rx-width-" + percentagePRB + @" animated"" role=""progressbar"" data-wow-duration=""0.5s"" data-wow-delay="".5s"" aria-valuenow=""0"" aria-valuemin=""0"" aria-valuemax=""100"" > ");
                        stringBuilder.Append(@"<span class=""photoshop-color"">").Append(valuePRB);
                        stringBuilder.Append("</span>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        break;

                    case 2:
                    case 8:
                        stringBuilder.Append(@"<div class=""single_experties"">");
                        stringBuilder.Append(@"<div class=""skilled-tittle"">").Append(GetMonthName(monthNumber));
                        stringBuilder.Append("</div>");
                        stringBuilder.Append(@"<div class=""progress"">");
                        stringBuilder.Append(@"<div class=""progress-bar jquery-bg wow Rx-width-" + percentagePRB + @" animated"" role=""progressbar"" data-wow-duration=""1s"" data-wow-delay="".15s"" aria-valuenow=""0"" aria-valuemin=""0"" aria-valuemax=""100"" > ");
                        stringBuilder.Append(@"<span class=""jquery-color"">").Append(valuePRB);
                        stringBuilder.Append("</span>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        break;

                    case 3:
                    case 9:
                        stringBuilder.Append(@"<div class=""single_experties"">");
                        stringBuilder.Append(@"<div class=""skilled-tittle"">").Append(GetMonthName(monthNumber));
                        stringBuilder.Append("</div>");
                        stringBuilder.Append(@"<div class=""progress"">");
                        stringBuilder.Append(@"<div class=""progress-bar php-bg wow Rx-width-" + percentagePRB + @" animated"" role=""progressbar"" data-wow-duration=""0.5s"" data-wow-delay="".25s"" aria-valuenow=""0"" aria-valuemin=""0"" aria-valuemax=""100"" > ");
                        stringBuilder.Append(@"<span class=""php-color"">").Append(valuePRB);
                        stringBuilder.Append("</span>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        break;

                    case 4:
                    case 10:
                        stringBuilder.Append(@"<div class=""single_experties"">");
                        stringBuilder.Append(@"<div class=""skilled-tittle"">").Append(GetMonthName(monthNumber));
                        stringBuilder.Append("</div>");
                        stringBuilder.Append(@"<div class=""progress"">");
                        stringBuilder.Append(@"<div class=""progress-bar html5-bg wow Rx-width-" + percentagePRB + @" animated"" role=""progressbar"" data-wow-duration=""0.5s"" data-wow-delay="".5s"" aria-valuenow=""0"" aria-valuemin=""0"" aria-valuemax=""100"" > ");
                        stringBuilder.Append(@"<span class=""html5-color"">").Append(valuePRB);
                        stringBuilder.Append("</span>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        break;

                    case 5:
                    case 11:
                        stringBuilder.Append(@"<div class=""single_experties"">");
                        stringBuilder.Append(@"<div class=""skilled-tittle"">").Append(GetMonthName(monthNumber));
                        stringBuilder.Append("</div>");
                        stringBuilder.Append(@"<div class=""progress"">");
                        stringBuilder.Append(@"<div class=""progress-bar css3-bg wow Rx-width-" + percentagePRB + @" animated"" role=""progressbar"" data-wow-duration=""1s"" data-wow-delay="".15s"" aria-valuenow=""0"" aria-valuemin=""0"" aria-valuemax=""100"" > ");
                        stringBuilder.Append(@"<span class=""css3-color"">").Append(valuePRB);
                        stringBuilder.Append("</span>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        break;

                    case 6:
                    case 12:
                        stringBuilder.Append(@"<div class=""single_experties"">");
                        stringBuilder.Append(@"<div class=""skilled-tittle"">").Append(GetMonthName(monthNumber));
                        stringBuilder.Append("</div>");
                        stringBuilder.Append(@"<div class=""progress"">");
                        stringBuilder.Append(@"<div class=""progress-bar marketing-bg wow Rx-width-" + percentagePRB + @" animated"" role=""progressbar"" data-wow-duration=""1.5s"" data-wow-delay="".25s"" aria-valuenow=""0"" aria-valuemin=""0"" aria-valuemax=""100"" > ");
                        stringBuilder.Append(@"<span class=""marketing-color"">").Append(valuePRB);
                        stringBuilder.Append("</span>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        stringBuilder.Append("</div>");
                        break;
                }
                if (rowNumber == 3 || rowNumber == 6 || rowNumber == 9 || rowNumber == 12)
                {
                    stringBuilder.Append("</div>");
                }
                rowNumber++;
            }

            divStartSelfPRB.InnerHtml = stringBuilder.ToString();
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

            GridViewTeamPRB.Columns[1].Visible = false; // Remove Emp ID
            GridViewTeamPRB.Columns[2].Visible = false; // Remove Team ID

            GridViewTeamPRB.DataSource = query;

            GridViewTeamPRB.DataBind();
            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "View Employee Profile successfully", "View Employee Profile successfully", Request.UserHostAddress, DateTime.Now);

        }
        catch (Exception Ex)
        {
            LogInfo.LockLogInfo(Session["EmpID"].ToString(), PageName, "View Employee Profile unsuccessfull", "View Employee Profile unsuccessfull", Request.UserHostAddress, DateTime.Now);
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
        pnlSelfPRB.Visible = false;
        pnlViewTeamPRB.Visible = false;
        pnlSetTeamPRB.Visible = true;

        btnSelfPRB.Attributes.Add("class", "button waves-effect default");
        btnViewTeamPRB.Attributes.Add("class", "button waves-effect default");
        btnSetTeamPRB.Attributes.Add("class", "button waves-effect default is-checked");
    }
}






<%@ Page Title="" Language="C#" MasterPageFile="~/HRConnectInnerMasterPage.master" AutoEventWireup="true" CodeFile="EmpPRB.aspx.cs" Inherits="EmpPRB" %>

<asp:Content ID="Content1" ContentPlaceHolderID="ContentPlaceHolder1" runat="Server">
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <!-- ==================== Emp-PRB-section start ==================== -->
    <div data-scroll='EmpPRB.aspx' class="my-portfolio-section sec-p100-bg-bs mb-30 clearfix" id="portfolio">
        <div class="Section-title" style="margin-bottom: -10px;">
            <h2>
                <i class="fa fa-pencil" aria-hidden="true"></i>Performance Reviews Bonus
            </h2>
            <span>Performance Reviews Bonus</span>
        </div>
        <!-- /.Section-title -->
        <div class="portfolio-area">
            <div id="filters" class="button-group">
                <asp:Button ID="btnSelfPRB" runat="server" Text="Self PRB" class="button waves-effect default is-checked" OnClick="btnSelfPRB_Click" />
                <asp:Button ID="btnViewTeamPRB" runat="server" Text="View Team PRB" class="button waves-effect default" OnClick="btnViewTeamPRB_Click" />
                <asp:Button ID="btnSetTeamPRB" runat="server" Text="Set Team PRB" class="button waves-effect default" OnClick="btnSetTeamPRB_Click" />

            </div>
            <div class="grid">
                <asp:Panel ID="pnlSelfPRB" runat="server">
                    <div class="skill_progress">
                        <div class="row" id="divStartSelfPRB" runat="server">
                        </div>
                        <!-- row -->
                    </div>
                </asp:Panel>
                <asp:Panel ID="pnlViewTeamPRB" runat="server">
                    <div class="skill_progress">
                        <div class="row" id="divStart" runat="server">

                            <asp:GridView ID="GridViewTeamPRB" runat="server" EnableViewState="false"
                                AutoGenerateColumns="false" ShowFooter="true" RowStyle-Wrap="false">
                                <FooterStyle BackColor="AntiqueWhite" ForeColor="Red" Font-Names="Arial, Helvetica, sans-serif"
                                    Font-Size="12px" Font-Bold="true" HorizontalAlign="Center" />
                                <Columns>
                                    <asp:TemplateField HeaderText="Sno">
                                        <ItemTemplate>
                                            <%#Container.DataItemIndex+1 %>
                                        </ItemTemplate>
                                    </asp:TemplateField>
                                    <asp:BoundField DataField="EmpID" HeaderText="EmpID" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="TeamID" HeaderText="TeamID" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="TeamName" HeaderText="Team Name" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="EmpName" HeaderText="Emp Name" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="January" HeaderText="Jan" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="February" HeaderText="Feb" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="March" HeaderText="Mar" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="April" HeaderText="Apr" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="May" HeaderText="May" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="June" HeaderText="June" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="July" HeaderText="July" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="August" HeaderText="Aug" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="September" HeaderText="Sept" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="October" HeaderText="Oct" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="November" HeaderText="Nov" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                    <asp:BoundField DataField="December" HeaderText="Dec" ItemStyle-HorizontalAlign="Center" ItemStyle-CssClass="label1" />
                                </Columns>
                                <EmptyDataTemplate>
                                    <div class="errot">
                                        <span>No Record Found.</span>
                                    </div>
                                </EmptyDataTemplate>
                            </asp:GridView>

                        </div>
                    </div>
                </asp:Panel>
                <asp:Panel ID="pnlSetTeamPRB" runat="server">
                    <div>div Set Team PRB div Set Team PRB div Set Team PRB div Set Team PRB div Set Team PRB div Set Team PRB div Set Team PRB div Set Team PRB</div>
                </asp:Panel>
            </div>
            <!-- /.grid -->
        </div>
        <!-- /.PRB-area -->
    </div>
    <!-- /.Emp-PRB-section -->
    <!-- ==================== Emp-PRB-section end ==================== -->
</asp:Content>

