# testtest


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
using System.Activities.Statements;

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
            var query = objmain.GetEmployeePRB((int)Session["EmpID"], (int)Session["Role"], DateTime.Now.Year, "Self").ToList();
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
        if (ddlEmpList.Items.Count == 0)
        {
            var query1 = objmain.TeamEmpDetails.Where(x => x.EmpID == (int)Session["EmpID"] && x.IsActive == 'A' && x.EmpDesignation == "TL").Select(x => new { x.TeamID }).Distinct();

            var query2 = (from ed in objmain.EmployeeDetails
                          join td in objmain.TeamEmpDetails on ed.EmpId equals td.EmpID
                          join q in query1 on td.TeamID equals q.TeamID
                          where ed.IsActive == 'A' && td.EmpDesignation != "TL"
                          select new
                          {
                              EmpID = ed.EmpId,
                              EmpName = ed.EmpName
                          }).ToList();

            ddlEmpList.DataSource = query2;
            ddlEmpList.DataTextField = "EmpName";
            ddlEmpList.DataValueField = "EmpID";
            ddlEmpList.DataBind();

            ddlEmpList.Items.Insert(0, new ListItem("-- Select Employee --", "00"));
        }
    }

    private void FillMonthList()
    {
        ddlMonth.Items.Clear();
        ddlMonth.Items.Insert(0, new ListItem("-- Select Month --", "00"));
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

    private bool Validate()
    {
        if (ddlEmpList.SelectedIndex == -1)
        {
            ScriptManager.RegisterStartupScript(Page, this.GetType(), "d", "alert('Select Employee ID.');", true);
            return false;
        }

        if (ddlMonth.SelectedIndex == -1)
        {
            ScriptManager.RegisterStartupScript(Page, this.GetType(), "d", "alert('Select Month.');", true);
            return false;
        }

        if (txtOverAllPRB.Text.Trim() == string.Empty)
        {
            ScriptManager.RegisterStartupScript(Page, this.GetType(), "d", "alert('Over all PRB can not be blank.');", true);
            return false;
        }
        return true;
    }

    protected void btnSave_Click(object sender, EventArgs e)
    {
        if (Validate())
        {
            using (DataClassesDataContext data = new DataClassesDataContext())
            {
                data.Connection.Open();
                using (data.Transaction = data.Connection.BeginTransaction())
                {
                    PRBDetail savePRB = new PRBDetail()
                    {
                        EmpID = Convert.ToInt32(ddlEmpList.SelectedItem.Value),
                        PRB = Convert.ToInt32(txtOverAllPRB.Text.Trim()),
                        PRBMonth = Convert.ToByte(ddlMonth.SelectedItem.Value),
                        PRBYear = Convert.ToInt32(ddlYear.SelectedItem.Value),
                        EnterDate = DateTime.Now,
                        GivenBY = Convert.ToInt32(Session["EmpID"].ToString()),
                        FeedBack = txtComment.Text.Trim(),
                        CreatedBY = Session["EmpID"].ToString(),
                        CreateDate = DateTime.Now,
                        CreateIPAddress = Request.UserHostAddress
                    };

                    data.PRBDetails.InsertOnSubmit(savePRB);
                    data.SubmitChanges();

                    int PRBId = data.PRBDetails.Select(C => C.ID).Max();
                    foreach (GridViewRow row in gridViewCriteria.Rows)
                    {
                        HiddenField hdnCriteriaID = (HiddenField)row.FindControl("hdnCriteriaID");
                        RadioButtonList rdoRating = (RadioButtonList)row.FindControl("rdoRating");
                        TextBox txtFeedback = (TextBox)row.FindControl("txtFeedback");

                        PRBCriteriaDetail savePRBCriteriaDetail = new PRBCriteriaDetail()
                        {
                            PRBID = savePRB.ID,
                            CriteriaID = Convert.ToInt32(hdnCriteriaID.Value),
                            Rating = Convert.ToByte(rdoRating.SelectedItem.Value),
                            FeedBack = txtFeedback.Text.Trim(),
                            CreatedBY = Session["EmpID"].ToString(),
                            CreateDate = DateTime.Now,
                            CreateIPAddress = Request.UserHostAddress
                        };
                        data.PRBCriteriaDetails.InsertOnSubmit(savePRBCriteriaDetail);
                    }
                    data.SubmitChanges();
                    data.Transaction.Commit();
                }
            }
        }
    }
}



/******************************************/

<asp:Panel ID="pnlSetTeamPRB" runat="server">
                    <div class="row">
                        <div class="contact-left clearfix">
                            <div class="select-dropdown">
                                <span class="focus-input100"></span>
                                <span class="symbol-input100">
                                    <i class="fa fa-user" aria-hidden="true">&nbsp;Employee</i>
                                </span>
                                <asp:DropDownList ID="ddlEmpList" runat="server" Style="display: block;" class="require">
                                </asp:DropDownList>
                            </div>
                            <div class="select-dropdown">
                                <span class="focus-input100"></span>
                                <span class="symbol-input100">
                                    <i class="fa fa-calendar" aria-hidden="true">&nbsp;Month</i>
                                </span>
                                <asp:DropDownList ID="ddlMonth" runat="server" Style="display: block;" class="require">
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
                                <asp:GridView ID="gridViewCriteria" runat="server" AutoGenerateColumns="false" CssClass="EU_DataTable">
                                    <Columns>
                                        <asp:TemplateField HeaderText="Sno">
                                            <ItemTemplate>
                                                <%#Container.DataItemIndex+1 %>
                                            </ItemTemplate>
                                        </asp:TemplateField>
                                        <asp:BoundField DataField="CriteriaName" HeaderText="Criteria" />
                                        <asp:TemplateField HeaderText="Rating">
                                            <ItemTemplate>
                                                     <asp:HiddenField ID="hdnCriteriaID" runat="server" Value='<%# Eval("CriteriaID") %>' />
                                                <asp:RadioButtonList ID="rdoRating" runat="server" RepeatDirection="Horizontal">
                                                    <asp:ListItem Value="5" Text="Excellent"></asp:ListItem>
                                                    <asp:ListItem Value="4" Text="Good"></asp:ListItem>
                                                    <asp:ListItem Value="3" Text="Average"></asp:ListItem>
                                                    <asp:ListItem Value="2" Text="Fair"></asp:ListItem>
                                                    <asp:ListItem Value="1" Text="Poor"></asp:ListItem>
                                                </asp:RadioButtonList>
                                                <asp:RequiredFieldValidator ID="RequiredFieldValidator1" runat="server" InitialValue="-1"
                                                    ControlToValidate="rdoRating" ForeColor="Red" ErrorMessage=" Rating field is required"></asp:RequiredFieldValidator>
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
                                <asp:ValidationSummary ID="ValidationSummary1" ForeColor="Red" runat="server" />
                            </div>

                            <div class="input-field">
                                <asp:TextBox ID="txtOverAllPRB" runat="server" class="require"></asp:TextBox>
                                <label>
                                    Over All PRB</label>
                            </div>
                            <div class="input-field">
                                <asp:TextBox ID="txtComment" runat="server" TextMode="MultiLine" class="materialize-textarea"></asp:TextBox>
                                <label>
                                    Comment</label>
                                <div class="response">
                                </div>
                            </div>

                            <%--<button type="button" class="submitForm custom-btn waves-effect">
                                <i class="fa fa-floppy-o" aria-hidden="true"></i>Save</button>--%>

                            <asp:ImageButton ID="btnSave" runat="server" Text="Save" ImageUrl="~/images/savebutton.png" OnClick="btnSave_Click" />
                        </div>

                        <!-- /.contact-right -->
                    </div>
                </asp:Panel>
