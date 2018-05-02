# iOS-Note



## Day3 - TableView 的下拉刷新和上拉刷新(swift)
#### （PS：本人很菜，但如果这些能对你有帮助，我也是很开心的）

我这里的调用了一个第三方库，SwiftFCXRefresh

这个库支持 pod 

	pod 'SwiftFCXRefresh'
	
然后在代码里

	import SwiftFCXRefresh

即可

### 在 Main.Storyboard 中，Embed in 一个 Navigation Controller，其他地方纯代码写

这里只是进行关键代码的粘贴


					
	import UIKit
	import SwiftFCXRefresh

	class ViewController: UIViewController {

	    var headerRefreshView: FCXRefreshHeaderView?
	    var footerRefreshView: FCXRefreshFooterView?
	   
	    var rows = 20
	    //let fullScreenSize = UIScreen.main.bounds.size
	    
	    let tableView = UITableView(frame: CGRect(x: 0, y: 0, width: UIScreen.main.bounds.size.width, height: UIScreen.main.bounds.size.height))
	    
	    override func viewDidLoad() {
	        
	        tableView.rowHeight = 100
	        tableView.dataSource = self
	        tableView.delegate = self
	        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "Cell")
	        
	        self.view.addSubview(tableView)
	        addRefreshView()
	        
	        super.viewDidLoad()
	        // Do any additional setup after loading the view, typically from a nib.
	    }
	    
	    override func viewDidAppear(_ animated: Bool) {
	        super.viewDidAppear(true)
	        headerRefreshView?.autoRefresh()
	    }
	
	    override func didReceiveMemoryWarning() {
	        super.didReceiveMemoryWarning()
	        // Dispose of any resources that can be recreated.
	    }
	    
	    
	    func addRefreshView() {
	        headerRefreshView = tableView.addFCXRefreshHeader { [weak self] (refreshHeader) in
	            self?.refreshAction()
	        }
	        footerRefreshView = tableView.addFCXRefreshFooter { [weak self] (refreshHeader) in
	            self?.loadMoreAction()
	        }
	
	    }
	    
	    //下拉刷新操作
	    func refreshAction() {
	        DispatchQueue.main.asyncAfter(deadline: .now() + 0.7) { [weak self] in
	            guard let weakSelf = self else {
	                return
	            }
	            weakSelf.rows = 20
	            weakSelf.footerRefreshView?.resetNoMoreData()
	            weakSelf.headerRefreshView?.endRefresh()
	            weakSelf.tableView.reloadData()
	        }
	    }
	    
	    //上拉加载更多
	    func loadMoreAction() {
	        DispatchQueue.main.asyncAfter(deadline: .now() + 0.7) { [weak self] in
	            guard let weakSelf = self,
	                let footerRefreshView = self?.footerRefreshView else {
	                    return
	            }
	            
	            weakSelf.rows += 10
	            
	            footerRefreshView.showNoMoreData()
	            
	            footerRefreshView.endRefresh()
	            /*
	            if  weakSelf.rows >= 29 {
	                //
	                footerRefreshView.showNoMoreData()
	            }
	            else {
	                footerRefreshView.endRefresh()
	            }
	             */
	            weakSelf.tableView.reloadData()
	        }
	        
	    }
    
	}
	
	extension ViewController: UITableViewDataSource, UITableViewDelegate {
	    
	    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath)
	        
	        cell.textLabel?.text = "\(indexPath.row)"
	        
	        return cell
	    }
	    
	    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
	        return rows
	    }
	}
	













