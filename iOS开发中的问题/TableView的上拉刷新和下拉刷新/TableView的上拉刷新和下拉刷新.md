# iOS-Note



## Day3 - TableView 的下拉刷新和上拉刷新(swift)

我这里的调用了一个第三方库，MJRefresh

这个库支持 pod 

	pod 'MJRefresh'
	
然后在代码里

	import MJRefresh

即可


这里只是进行关键代码的粘贴


	let footer = MJRefreshBackNormalFooter()
    let header = MJRefreshNormalHeader()

	
    func createRefresh() {
        header.setRefreshingTarget(self, refreshingAction: #selector(LIfeServeViewController.headerRefresh))
        
        header.setTitle("下拉刷新", for: .idle)
        header.setTitle("松开刷新", for: .pulling)
        header.setTitle("刷新中", for: .refreshing)
        header.lastUpdatedTimeLabel.isHidden = true
        
        self.lifeServeTableView!.mj_header = header
        
        
        footer.setRefreshingTarget(self, refreshingAction: #selector(LIfeServeViewController.footerLoad))
        //是否自动加载（默认为true，即表格滑到底部就自动加载）

        footer.setTitle("上拉加载", for: .idle)
        footer.setTitle("松开加载", for: .pulling)
        footer.setTitle("加载中", for: .refreshing)

        self.lifeServeTableView!.mj_footer = footer
        
        header.beginRefreshing()
    }
    
    
    @objc func headerRefresh(){
 
    }
    
    
    @objc func footerLoad(){
        
        
    }
    











