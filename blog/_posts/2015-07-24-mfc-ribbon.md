---

layout: post

title: mfc ribbon

author: ilsec

---

##禁用CMFCRibbonBar的QATMenu

1. 继承*CMFCRibbonBar*
2. 重载*OnShowRibbonQATMenu*

代码如下

	// 接口
	class CILRibbonBar : public CMFCRibbonBar
	{
		DECLARE_DYNCREATE(CILRibbonBar)
	public:
		virtual BOOL OnShowRibbonQATMenu(CWnd* pWnd, int x, int y, CMFCRibbonBaseElement* pHit);
	};

	// 实现
	IMPLEMENT_DYNCREATE(CILRibbonBar, CMFCRibbonBar)
	BOOL CILRibbonBar::OnShowRibbonQATMenu(CWnd* pWnd, int x, int y, CMFCRibbonBaseElement* pHit)
	{
		return FALSE;
	}

##禁用CMFCRibbonApplicationButton

1. 继承*CILRibbonApplicationButton*
2. 重载*OnLButtonDown*
3. 重载*OnLButtonDblClk*

代码如下

	// CILRibbonApplicationButton 类的接口
	//
	class CILRibbonApplicationButton : public CMFCRibbonApplicationButton
	{
		DECLARE_DYNCREATE(CILRibbonApplicationButton)
	public:
		virtual void OnLButtonDown(CPoint point);
		virtual void OnLButtonDblClk(CPoint point);
	};

	// CILRibbonApplicationButton 类的实现
	//
	IMPLEMENT_DYNCREATE(CILRibbonApplicationButton, CMFCRibbonApplicationButton)
	void CILRibbonApplicationButton::OnLButtonDown(CPoint point)
	{
	}
	void CILRibbonApplicationButton::OnLButtonDblClk(CPoint point)
	{
	}

注意的一点

	1. m_wndRibbonBar.SetApplicationButton 的参数是分配的堆内存而不是栈。
	2. m_wndStatusBar 创建之前修改ApplicationButton。

	CILRibbonApplicationButton *m_pMainButton = new CILRibbonApplicationButton();
	m_pMainButton->SetImage(IDB_MAIN);
	m_wndRibbonBar.SetApplicationButton(m_pMainButton, CSize(24, 24));	//m_MainButton.SetImage(IDB_MAIN);
	//m_wndRibbonBar.SetApplicationButton(&m_MainButton, CSize(24, 24));
	if (!m_wndStatusBar.Create(this))
	{
		TRACE0("未能创建状态栏\n");
		return -1;      // 未能创建
	}