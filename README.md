(defun c:yhd()
	(fz000)
	(setq jidian ( list 4000 ( + h3 2000 1500)))	;4500 1500 过内边框 ，25 +20 又加上5 以使标注线在图框内。
	
	(zdm_zbd)
	(zdm_dm)
	(pingm)
	
	
	(setq jidian ( list 3500 ( + h3 2000 1500)))	;4500 1500 过内边框 ，25 +20 又加上5 以使标注线在图框内。
	
	(hdmfuzi)
	(hdm_dm)
		(command "clayer" "2")
	(command "rectang" "47000,0" "@42000,29700" );A3 on 1:100 print,truse 
	(command "CLAYER" "3"	"lweight" "bylayer");设当前图层为3,线宽随层
	(command "rectang" "49500,1000" "@38500,27700" )
	(setq jidian ( list 95000  ( + h3 2000 1500)))	;4500 1500 过内边框 ，25 +20 又加上5 以使标注线在图框内。
	(zdm_gj)
	(hdm_gj)
	(gj)
	(gjbfuzi)
	
	
	(prompt "*****溢洪道设计*****辉南通润*****")
	(prin1)
	(setvar "osmode" 36)			;
)

(defun fz000 ()
	
	(if (null dcl_pt)			;预览对话框出现在界面中央	
		(setq dcl_pt '(-1 -1))	; 
	)
	(setq dcl_file "D:\\yhdlisp\\dcl\\dia00.dcl");(getfiled "打开 DCL 文件" "D:\\yhdlisp\\dcl\\dia00.dcl" "DCL" 2))
	;(setq dia_name (getstring "\nDialog 对话框名:"))
	;(if (= dia_name "")(exit))
	(setq dcl_id (load_dialog dcl_file))
	
	(new_dialog "dia00" dcl_id)	;启动dia5a 对话框 
	(set_tile "edit_l1" "3000")
	(set_tile "edit_l2" "4000")
	(set_tile "edit_l3" "3000")
	(set_tile "edit_l4" "10000")
	(set_tile "edit_h1" "1100")
	(set_tile "edit_h2" "2000")
	(set_tile "edit_h3" "3000")
	(set_tile "edit_h4" "1200")
	(action_tile "accept" "(ok_dia00) (done_dialog 1)");ok_dia00是选择后执行的函数。注意：各变量需求的数据类型不同，所以须再用字符串转整数atoi函数或字符串转实数atof函数转换。当以 action tile函数选择“确定”按钮(key值=" accept")后，直接调用执行( ok dia00)子程序，并以done_ dialog函数退出对话框执行
	(action_tile "cancel" "(done_dialog) (setq result nil)" )
  ;关闭按钮被点击
	(start_image "img_cr")
	(slide_image 0 0 (dimx_tile "img_cr") (dimy_tile "img_cr") "D:\\yhdlisp\\sld\\zdm.sld")
	(end_image)
	(start_dialog)
	(setvar "osmode" 0)			;不捕捉(setvar "osmode" 36)	端点 中点 交点
	(command "layer" "m" "1" "c" "3" "" "LT" "dashed" "" "");设虚线图层
	(command "layer" "m" "2" "c" "4" "" "LT" "CONTINUOUS""" "");设细实线图层,注意这个”contionus后面的“2”是图层名
	(command "layer" "m" "3" "c" "4" "" "LT" "CONTINUOUS" "2" "lw" 0.4 "" "");设粗实线图层,注意这个”contionus后面的“2”是图层名
	
	(setq hgj 1000)			;hgj高
  (setq houdbgj 500)			;底板厚度
  (setq houqiangj 300)		;边墙厚度
  (setq juli-tu 5000);设图块的距离 是5000
	
	
	
  (setq h0 500)
	
	(setq b2 2000)  ;出口一字墙长。
  (setq bili 100)			; (getint "绘图比例"))
	
	(setq jjgj 200)
	(setq hbaohu 50)			;保护层厚
  (setq hdpl 0.5)			;多段线宽
  (setq rgj 55) ;点钢筋外径;(setq jidian2 (getpoint "jidan:"))
  (setq ls (+ hbaohu (/ hdpl 2)))	;线宽的一半加上保护层
  (setq ls1 (+ hbaohu hdpl (/ rgj 2)))
  
  (setq A3 (atan (/ h3 (- l3 500) 1.00)))	;指斜坡段的角度
	
	(setq h-qmb 250)			;桥面板厚
	(setq b 3000) ;(getint "溢洪道宽"))
	(setq m_bp 2);设上游坝坡比为2********以后设输入框
	(setq h101 (/ (- (* m_bp (+ h2 500 250)) (- l1 300)) m_bp));进口墙，按长l1计算。其中250 是桥面板厚度，可代入
	(setq a_bzq (/ pi 6.000));平面图八字墙 角度，30度
	(setq a_tang (/ (sin a_bzq) (cos a_bzq)))
	(setq l0 (* m_bp (+ h2 hd_qmb)));l0 计算得来的，上一函数直接给不
	(command "clayer" "2" "")
	(command "rectang" "0,0" "@42000,29700" );A3 on 1:100 print,truse 
	(command "clayer" "3" "")
	(command "rectang" "2500,1000" "@38500,27700" )
)
(defun ok_dia00();dialog run 
	(setq l1  (atoi (get_tile "edit_l1")))
	(setq l2  (atoi (get_tile "edit_l2")))
	(setq l3  (atoi (get_tile "edit_l3")))
	(setq l4  (atoi (get_tile "edit_l4")))
	(setq h1  (atoi (get_tile "edit_h1")))
	(setq h2  (atoi (get_tile "edit_h2")))
	(setq h3  (atof (get_tile "edit_h3")))
	(setq h4 (atoi (get_tile "edit_h4")))
	(setq m_bp  (atof (get_tile "edit_m_bp")))
	(setq b  (atoi (get_tile "edit_b")))	
	(setq a_bzq (atof (get_tile "edit_a_bzq")))	
	(setq hd_qmb  (atof (get_tile "edit_hd_qmb")))	
	
)
(defun zbjs 	(pt1  ptx pty)
	;********
	;点坐标计算，计算XY的增量，计算式简化了
	;********
  (setq pt (mapcar '+ pt1 (list ptx pty )))
)
(defun zbth (pt ptx pty)
	;pt point ptx pty 为要替换的点x y 坐标。如果一个为零就不用换了，换不为零的哪个。
	(if (= 0 ptx)  (setq linpoint (list (car pt)  pty)))
	(if (= 0 pty)  (setq linpoint (list ptx  (cadr pt))))
  (setq pt linpoint)
);*****zbth finish
(defun tukuang ()
	(command "clayer" "2")
	(command "rectang" pt1 "@42000,29700" );A3 on 1:100 print,truse 
	(setq pt1 (zbjs pt1 2500 1000))
	(command "CLAYER" "3"	"lweight" "bylayer");设当前图层为3,线宽随层
	(command "rectang" "49500,1000" "@38500,27700" )
		);*****tukuang finish
(defun zd	(pt1 pt2);calculate mid 
  (setq zdl (/ (distance pt1 pt2) 2))
  (mapcar
		'+
		pt1
		(list
			(/ (- (car pt2) (car pt1)) 2)
			(/ (- (cadr pt2) (cadr pt1)) 2)
		)
  ) 	;由PTPT2求中点坐标。
)
(DEFUN zdm_zbd	( )
	(setq pzd101 jidian
		pzd102 (zbjs pzd101 0 h1) 
		pzd103 (zbjs pzd102 0 h101) 
		pzd104 (zbjs pzd103 300 0) 
		pzd1011 (zbjs pzd101 300 0)
		pzd1010  (zbjs pzd1011 300 600)
		pzd109 (zbjs pzd1010  (- l1 1200) 0)
		pzd108 (zbjs  pzd109 300 -600)
		pzd107 (zbjs pzd108  300 0)
		pzd106 (zbjs pzd107 0 h1)
		pzd105 (zbjs pzd106 0 (+ h2 250 500) ) 
	);进口渠段
	(setq pzd201	(zbjs 	pzd101 l1 0)
		pzd202	(zbjs 	pzd201 0 (+ h1 500) )
		pzd203	(zbjs 	pzd202 0 h2)
		pzd204	(zbjs 	pzd203 0 500)
		pzd205	(zbjs 	pzd204 250 0)
		pzd206	(zbjs 	pzd205 0 -250)
		pzd207	(zbjs 	pzd206 (- l2 500)  0)
		pzd208	(zbjs 	pzd207 0 250)
		pzd209	(zbjs 	pzd208 250 0)
		pzd2010	(zbjs 	pzd209 0 -500)
		pzd2011	(zbjs 	pzd2010 0 (* -1 h2))
		pzd2012	(zbjs 	pzd2011 0 (- -500 h1))
		pzd2016	(zbjs 	pzd201 300 0)
		pzd2015	(zbjs 	pzd2016 300 900)
		pzd2014	(zbjs 	pzd2015 (- l2 1200) 0)
		pzd2013	(zbjs 	pzd2014 300 -900)
		pzd2012	(zbjs 	pzd2013 300 0)
	);溢流段完成
	(setq pzd301	pzd2012
		pzd302	(zbjs 	pzd301 0 (+ h1 500))
		pzd303	(zbjs 	pzd302 0 (+ h2 250))
		pzd304	(zbjs 	pzd303 500 0)
		pzd305	(zbjs 	pzd304 (- l3 500) (* -1 (- (+ h2 h3 250) h4)))
		pzd306	(zbjs 	pzd305 0 (* -1 h4))
		pzd307	(zbjs 	pzd306 0 -1600)
		pzd308	(zbjs 	pzd307 -300 0)
		pzd309	(zbjs 	pzd306 -600 (* -1 (- (/ 500 (cos A3 )) (* 600 (/ (sin A3) (cos A3))))))
		pzd3010	(zbjs 	pzd309  (* -1 (- l3 600 600))  (* (- l3 600 600) (/ (sin a3) (cos a3))))
		pzd3011	(zbjs 	pzd301 300 0)	
		pzd3012	(zbjs 	pzd302 600 0)	;斜坡段捌 点，也可以是500			
	)
	(setq pzd401	pzd307
		pzd402	(zbjs 	pzd401 0 1600)
		pzd403	(zbjs 	pzd402 0 h4)
		pzd404	(zbjs 	pzd403 l4 0)
		pzd405	(zbjs 	pzd404 0 (* -1 (- h4 500)))
		pzd406	(zbjs 	pzd405 -500 0)
		pzd407	(zbjs 	pzd406 0 -500)
		pzd408	(zbjs 	pzd407 500 0)
		pzd409	(zbjs 	pzd408 0 -1600)
		pzd4010	(zbjs 	pzd409 -300 0)
		pzd4011	(zbjs 	pzd4010 -300 1100)
		pzd4013	(zbjs 	pzd401 300 0)
		pzd4012 (zbjs 	pzd4013 300 1100)	
	)
)
(defun zdm_dm()
	(command "clayer" "3" )
	(command "pline" pzd101 pzd102 pzd103 pzd104 pzd105 pzd106 pzd107 pzd108 pzd109 pzd1010 pzd1011 pzd101 "")	
	(command "pline" pzd102 pzd106 "")			
	(command "pline" pzd201 	pzd202 	pzd203	pzd204	pzd205	pzd206	pzd207	pzd208	pzd209	pzd2010 pzd2011 pzd2012 pzd2013 pzd2014 pzd2015 pzd2016 pzd201 "" )
	(command "pline" pzd203 pzd2010 "")
	(command "pline" pzd202 pzd2011 "")
	(command "pline" pzd301 	pzd302 	pzd303	pzd304	pzd305	pzd306	pzd307	pzd308	pzd309	pzd3010 pzd3011 pzd301 "" )
	(command "pline" pzd302 	pzd3012 	pzd306 "")
	(command "pline" pzd401 	pzd402 	pzd403	pzd404	pzd405	pzd406	pzd407	pzd402	"")
	(command "pline" pzd401 	pzd4013 	pzd4012		pzd4011 pzd4010	pzd409	pzd405 "")			
	(command "clayer" "2")
	
	(command "zoom" "a")
	(command "dimtxt" 200)		;标注文字高度2.5
	(command "DIMASZ" 100)		;箭头大小4-5
	(command "DIMDLI" 800)		;尺寸界线间距8
	(command "DIMEXE" 300)		; 尺寸界线超出量3-4
	(command "DIMEXO" 150)		;尺寸界线间隙,具体要看标注点的位置，比如，下面@后的数，要大于这个才会显示距离出来
	(command "DIMlfac" 0.1)		;尺寸测量比例
	(command "dimdec" 0);数据小数位 
	(command "dimdsep" ".");小数点应是。号，不是，
	(setq pty (cadr pzd409))
	(command "dimlinear" pzd409 pzd4010 "h" "@500,-500" "");
	(command "dimcontinue" pzd4011 pzd4012 pzd4013 pzd401 pzd308 (zbth pzd309 0 pty)  (zbth pzd3010 0 pty) (zbth pzd3011 0 pty) (zbth pzd301 0 pty)  (zbth pzd2013 0 pty) (zbth pzd2014 0 pty) (zbth pzd2015 0 pty) (zbth pzd2016 0 pty) (zbth pzd201 0 pty)  (zbth pzd108 0 pty) (zbth pzd109 0 pty) (zbth pzd1010 0 pty) (zbth pzd1011 0 pty) (zbth pzd10` 0 pty) "" ""  );
	(command "dimlinear" pzd409 pzd408 "v" "@500,-500" "");
	(command "dimcontinue" pzd405 pzd404 ""  "")
	(setq ptx (car pzd101))
	(command "dimlinear" pzd101 pzd102 "v" "@-500,-500" "");
	(command "dimcontinue" pzd103 (zbth pzd105 ptx 0) (zbth pzd204 ptx 0) "" "")
	(command "dimlinear" pzd204 pzd205 "h" "@-500,500" "")
	(command "dimcontinue" pzd208  pzd209 "" "") 
	(command "dimlinear" pzd303 pzd304 "h" "@-500,500" "")
	(command "dimlinear" pzd407 pzd406 "v" "@-500,500" "")
	(command "dimlinear" pzd406 pzd405 "h" "@-500,500" "")
	
	(command "dimlinear" pzd1011 pzd1010 "v" "@500,500" "")
	(command "dimlinear" pzd2016 pzd2015 "v" "@500,500" "")
	(command "dimcontinue" pzd202 pzd203 pzd206  pzd205 "" "")
	(command "dimlinear" pzd3011 pzd3010 "v" "@500,500" "")
	(command "dimlinear" pzd4013 pzd4012 "v" "@500,500" "")
	(command "dimcontinue" pzd402 pzd403 (zbth pzd3012 (car pzd4013) 0) "" "")
	(command "dimlinear" pzd409 pzd405 "v" "@500,500" "")
	(command "dimcontinue" pzd404 (zbth pzd304 (car pzd409) 0) "" "" )
	;如果标注被填充盖上，dimtfill,将默认值<0>改为1.先标注后填充可能不受此限制。不知道。
	(command "hatch" "ansi31" 20 0 "" "n"  pzd101 pzd102 pzd106 pzd107 pzd108 pzd109 pzd1010 pzd1011 pzd101 "" "")
	(command "hatch" "ar-conc" 5 0 "" "n" pzd101 pzd102 pzd106 pzd107 pzd108 pzd109 pzd1010 pzd1011 pzd101 "" "")
	;输入图案名或 [?/实体(S)/用户定义(U)] <ANGLE>: GRAVEL~~~~~指定图案缩放比例 <1.0000>: 10~~~~~指定图案角度 <E>: 0 ~~~~~选择定义填充边界的对象或 <直接填充>(此处选择对象:/"")~~~~~是否保留多段线边界？[是(Y)/否(N)] <N>: n~~~~~指定起点: ~~~~~要点或窗口(W)/上一个(L)/窗交(C)/框(BOX)/全部(ALL)/栏选(F)/圈围(WP)/圈交(CP)/编组(G)/添加(A)/删除(R)/多个(M)/前一个(P)/放弃(U)/自动(AU)/单个(SI)
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs  pzd201 100 100) "" "n" "");第二方法填充，这个用点少。tmd 用了四五天，就没明白，原来是 20，0 上面都得有引号。tmd，tdmd。指定内部点才行，真真他，，才明白。	bhatch,hatch,tmd qubie ?????
	(command "bhatch" "p" "ar-conc" "5" "0" (zbjs  pzd201 100 100) "" "n" "");tmd tmd tmd tmd tmd 
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs  pzd301 100 100) "" "n" "");
	(command "bhatch" "p" "ar-conc" "5" "0" (zbjs  pzd301 100 100) "" "n" "");
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs  pzd401 100 100) "" "n" "");
	(command "bhatch" "p" "ar-conc" "5" "0" (zbjs  pzd401 100 100) "" "n" "");
)
(defun pingm ()
	
	(command "celtscale" 500);当前线型比例，設定新物件相對於 LTSCALE 指令設定的線型比例。在 LTSCALE 設為 0.5 的圖面中，以 CELTSCALE = 2 來建立的線條，其外觀如同在 LTSCALE 設為 1 的圖面中，以 CELTSCALE = 1 來建立的線條。
	
	(setq pm101  (zbjs pzd204 (* -1 l1) 3000)
		pm102 (zbjs pm101 0 1000)
		pm103 (zbjs pm102  l1 (* l1 (/ (sin a_bzq) (cos a_bzq))))
		pm104 (zbjs pm103 0 (* -1 (/ 300 (cos a_bzq))))
		pm105 (zbjs pm104 (* -1 (- l1 300)) (* -1 (- l1 300) (/ (sin a_bzq) (cos a_bzq)) ))
		pm106 (zbjs pm101 300 0)
		pm109 (zbjs pm104 0 (* -1 (/ 200 (cos a_bzq))))
		pm108 (zbjs pm109 (* -1 (- l1 500)) (* -1 (- l1 500) (/ (sin a_bzq) (cos a_bzq))))
		pm107 (zbjs pm106 200 0)
	  pm201 pm103
		pm202 (polar pm201 0 l2)
		pm203 (zbjs pm202 0 -500)
		pm204  (zbjs pm201 0 -500)
	  pm301 pm202
		pm302 (zbjs pm301 l3 0)
		pm401 pm302
		pm402 (zbjs pm401 (- l4 500) 0)
		pm403 (zbjs pm402 500 0)
		pm404 (zbjs pm403 0 -2000)
	)
	(command "clayer" "3" "lweight" "bylayer")
	(command "line" pm101 pm102 pm103 "'Clayer"  "1" pm202 "'Clayer"  "3"  pm302 pm403 "@0,-2000" "@-300,0" "@0,1700" (zbjs pm301 0 -300) pm203 pm204  pm104 pm105 pm106 pm101 "")
	(command "clayer" "1" )
	(command "line" pm106 pm107  pm108 pm109 pm204 "")
	(command "line" pm203 (zbjs pm203 (+ l3 l4 -500) 0) (zbjs pm404 -500 0) pm404 "")
  (command "line" pm105  pm108 "")
	
	
	
	
	(setq pmx01 (polar pm204 0 (- (/ l2 2) 150)) 
		pmx02 (zbjs pmx01 300 0) 
		pmx03 (zbjs pmx02 0 -2000) 
		pmx04 (zbjs pmx01 0 -2000));设心墙的四个角点。
	(command "pline" pmx01 pmx02 pmx03 pmx04  pmx01 "");画心墙
	
	(setq pmb01 (polar pm204 (/ pi -2)  2300));此处2300是为了让线长大于心墙的2000 长。指坝 坡上的线，
	(setq pmb02 (polar pmb01 0  l2));
	
	(command "clayer" "3");绘坝顶，和折断线
	(command "pline" pmb01 pm204 (zbjs pm204 0 (+ (/ b 2) 500)) "")
	(command "pline" pmb02 pm203  "")
	(command "clayer" "2")
	(command "pline" (polar pmb01 0 -300) (strcat "@" (rtos (/ l2 3 )  2 2 ) ",0" ) (strcat "@" (rtos 300  2 2 ) "<45" )  (polar (polar pmb01 0 (+ 300 (/ l1 3))) (* 5 (/ pi 4)) 300) (polar pmb01 0 (+ 300 (/ l1 3)))  pmb02 "@300,0" ""  );折断线完成。
	
	;以下为画分段的线
	(command "pline" "'Clayer"  "3" (zbjs pm204 250 0)  (strcat "@0," (rtos (+ (/ b 2) 500) 2 2) ) "" );台帽线，上游
	(command "offset" (- l2 500) (entlast) pm203 "")
	(command "offset" 250 (entlast) pm203 "")
	
	
	(command "line" (zbjs pm302 0 -300) (zbjs pm302 0 (/ b 2)) "");陂坡与消力池交线
	(command "pline" pm403 (zbjs pm403 0 (/ b 2)) "");
	(command "pline" (zbjs pm403 -500 0) (zbjs pm403 -500 (/ b 2)) "");
	
	
	(command "clayer" "1")
	(command "pline" (zbjs pm302 0 -300)  (zbjs pm302 0 -500) "" );陡坡与消力的外斜墙虚线。	
	
	
	
	
	
	(setq zxd01  (zbjs pm201 0 (/ b 2)) 
		zxd02 (zbjs pm202 0 (/ b 2))  );also ;set many zxd for mirror .设镜像线的两点坐标。
	( command"zoom" "a");为确保选到指定的图元，显示整个作图范围。
	(command "mirror" "c" (zbjs pm101 -1000 -1000) (zbjs pm403 1000 b) "" zxd01 zxd02  "n");用后面的公式不准确。还是平移pm02点准。pm27 (/ pi 2) (+ b2 (/ b 2))) 
	
	(setq pty (cadr pm101))	
	(command "clayer" "2")
	(command "dimlinear" pm101 pm106  "h" "@500,-500" "");
	(command "dimcontinue" pm107  "" "");连续标注的退出是用两个“”
	(command "dimlinear" pmx03 pmx04  "h" "@500,-500" "");
	(command "dimlinear" pmx01 pmx03  "v" "@-500,-500" "");
	(command "dimlinear" pm404 (zbjs pm404 -300 0) "h" "@-500,-500" "");
	(command "dimcontinue"  (zbjs pm404 -500 0) "" "")
	(command "dimlinear" pm101 (zbth pm201 0 pty) "h" "@-500,-1000" "");
	(command "dimcontinue"  (zbth pm301 0 pty)  (zbth pm401 0 pty)  (zbth pm403 0 pty) "" "")
	(command "dimlinear" pm404 pm403 "v" "@500,-500" "")
	(command "dimcontinue"  (zbjs pm403 0 b)   (zbjs pm403 0 (+ b 2000)) "" "")
	(setq ptx (car pm101))
	(command "dimlinear" pm101 pm102 "v" "@-500,-500" "")
	(command "mirror" (entlast) "" zxd01 zxd02  "n")
	(command "dimlinear" pm102 (zbth pm103 ptx 0)  "v" "@-500,-500" "")
	(command "mirror" (entlast) "" zxd01 zxd02  "n")
	(command "dimlinear" (zbth pm103 ptx 0) (strcat "@0," (rtos b 2 0 ) ) "v" "@-500,-500" "")
	;(command "dimlinear" pm102 "@0,2000"  "v" "@-500,-500" "")
	
	
	
	
)

(defun hdmfuzi ()
	(setq pdm101 jidian );分在图在上部分，见详图
	(setq pdm102 (zbjs pdm101 0 600)
		pdm103	(zbjs 	pdm102 0 500)
		pdm104	(zbjs 	pdm103 0 500)
		pdm105	(zbjs 	pdm104 0 500)
		pdm106	(zbjs 	pdm105 300 0)
		pdm107	(zbjs 	pdm103  500 0)
		pdm108	(zbjs 	pdm102  500 0)
		pdm109	(zbjs 	pdm101  300 0)
	)
	;**** 2
	(setq pdm201 (polar pdm108 0 3000); finish del***第二个断面 同第一个距离	
		pdm202  	(zbjs 	pdm201  0 500)
		pdm203	(zbjs 	pdm202  (/ 200 (cos a_bzq)) h101)
		pdm204	(zbjs 	pdm203 (/ 300 (cos a_bzq)) 0)
		pdm205	(zbjs 	pdm204 0 (* -1 h101))
		pdm206	(zbjs 	pdm205 (+ (* l1 (/ (sin a_bzq) (cos a_bzq))) b) 0)
		pdm20z  (zbjs pdm205 (+ (* l1 (/ (sin a_bzq) (cos a_bzq))) (/ b 2)) 0)
		pdm207	(zbjs 	pdm206 0 (+ h2 500 250))
		pdm208	(zbjs 	pdm207 (/ 300 (cos a_bzq)) 0)
		pdm209	(zbjs 	pdm208 (/ 200 (cos a_bzq)) (* -1 (+ h2 500 250)) )
		pdm2010	(zbjs 	pdm209 0 -500)
	)
	
	
	
	;******3
	
	;pdm302 此处是指防渗墙高，但是几个事情，h0 hd_qmb 500 1000 是底板的厚， 以后要代入变量，不是数据 。此处为了简便。
	(setq pdm301 (zbjs pdm2010 3000 0)
		pdm302	(zbjs  	pdm301  0 (+ h2 700 h-qmb))
		pdm303	(zbjs  	pdm302 0 250)
		pdm304	(zbjs  	pdm303 (+ b 500 500) 0)
		pdm305	(zbjs  	pdm304 0 -250)
		pdm306	(zbjs  	pdm301 (+ b 500 500) 0 )
		pdm307	(zbjs  	pdm301 500 700)
		pdm308	(zbjs  	pdm307 0 h2)
		pdm309	(zbjs  	pdm308 b 0)
		pdm3010	(zbjs  	pdm307 b 0)
		
	)
	
	
	
	;*********4 桥面板厚h-qmd按250，台帽按250高，暂定，以后改。
	
	
	(setq pdm401	(zbjs  	pdm306 3000 0)
		pdm402	(zbjs  	pdm401 0 500)
		pdm403	(zbjs  	pdm402 200 (+ h2 250))
		pdm404	(zbjs  	pdm403 300 0)
		pdm405	(zbjs  	pdm402 500 0)
		pdm406	(zbjs  	pdm405 b 0)
		pdm407	(zbjs  	pdm406 0 h4)
		pdm408	(zbjs  	pdm407 300 0)
		pdm409	(zbjs  	pdm406 500 0)
		pdm4010	(zbjs  	pdm409 0 -500)
		
	)
	
	
	;此处为斜坡处断面，如断面前后高度不一至，则断面不对，不做太复杂 考虑。只是调整 一下割断面处的高度就行了，l
	(setq pdm501	(zbjs  	pdm4010 3000 0)
		pdm502	(zbjs  	pdm501 0 500)
		pdm503	(zbjs  	pdm502 200 h4)
		pdm504	(zbjs  	pdm503 300 0)
		pdm505	(zbjs  	pdm502 500 0)
		pdm506	(zbjs  	pdm505 b 0)
		pdm507	(zbjs  	pdm506 0 h4)
		pdm508	(zbjs  	pdm507 300 0)
		pdm509	(zbjs  	pdm506 500 0)
		pdm5010	(zbjs  	pdm501 (+ 500 b 500) 0)
	)
	
	
	;**********以下是6断面 	
	(setq pdm601 (polar pdm5010 0 3000)
		pdm602	(zbjs  	pdm601 0 1100)
		pdm603	(zbjs  	pdm602 0 500)
		pdm604	(zbjs  	pdm603 0 500)
		pdm605	(zbjs  	pdm604 0 (- h4 500))
		pdm606	(zbjs  	pdm605  300 0)
		pdm607	(zbjs  	pdm603 500 0)
		pdm608	(zbjs  	pdm602 500 0)
		pdm609	(zbjs  	pdm601 300 0)
	)
	
	(setq	pdm701	(polar pdm609 0 3000  )	
		pdm702	(zbjs  	pdm701 0 (+ 700 h2 250))
		pdm703	(zbjs  	pdm702 300 0)
		pdm704	(zbjs  	pdm701 300 0)	)
	
);dmfuzi finish

(defun hdm_dm()
	(command "clayer" "2")
	(command "rectang" "47000,0" "@42000,29700" );A3 on 1:100 print,truse 
	(command "CLAYER" "3"	"lweight" "bylayer");设当前图层为3,线宽随层
	(command "rectang" "49500,1000" "@38500,27700" )
	
	(hdmfuzi)
	(command "clayer" "3")
	
	(command "pline" pdm101 pdm105 pdm106 pdm107 pdm108  pdm109 pdm101 "")
	(command "pline" pdm201 pdm202  pdm203 pdm204 pdm205 pdm206 pdm207 pdm208 pdm209 pdm2010  pdm201 "")
	
	
	(command "pline" pdm301 pdm302 pdm303 pdm304 pdm305  pdm306  pdm301   "")
	(command "pline" pdm302 pdm305   "") 
	(command "pline" pdm307 pdm308 pdm309  pdm3010  pdm307      "")
	
	(command "pline" pdm401 	pdm402	pdm403	pdm404	pdm405	pdm406	pdm407	pdm408	pdm409 pdm4010 pdm401 "")
	(command "pline" pdm501 	pdm502	pdm503	pdm504	pdm505	pdm506	pdm507	pdm508	pdm509	pdm5010 pdm501  "")
	(command "pline" pdm601 	pdm605	pdm606	pdm607	pdm608	pdm609	pdm601 "")
	(command "pline" pdm701 pdm702 pdm703 pdm704 pdm701 "")
	(command "zoom" "a")
	
	
	(command "clayer" "2")
	(command "dimlinear" pdm101 pdm102 "h" "@-500,-500")	
	(command "dimlinear" pdm106 pdm109 "v" "@1000,500")
	(command "dimlinear" pdm106 pdm107 "v" "@500,500")
	(command "dimcontinue" pdm108 pdm109 "" "") 
	
	(command "dimlinear" pdm201 pdm2010 "h" "@-500,-500")
	(command "dimlinear" pdm201 pdm202 "v" "@-500,-500")
	(command "dimcontinue" (zbth pdm203 (car pdm201) 0 )""  "")
	;桥面板标注
  (command "dimlinear"  (zbth pdm201 0 (cadr pdm203)) 		pdm203 "h" "@-500,500" ) 
	(command "dimcontinue" pdm204 (zbth pdm20z 0 (cadr pdm203) )""  "")
	(command "dimlinear"  (zbth pdm20z 0 (cadr pdm207) ) 	pdm207 "h" "@-500,500" ) 
	(command "dimcontinue" pdm208 (zbth pdm209  0 (cadr pdm203) )""  "")
	(command "dimlinear" pdm2010	pdm209 "v" "@500,500" ) 
	(command "dimcontinue" (zbth pdm208  (car pdm209)  0 ) ""  "")
	;桥面板标注
	
	(command "dimlinear"  pdm301 pdm302 "v" "@-500,500" ) 
	(command "dimcontinue"  pdm303 ""  "")
	(command "dimlinear"  pdm301 pdm303 "v" "@-1000,500" ) 
	(command "dimlinear"  pdm301 (zbth pdm307 0 (cadr pdm301)) "h" "@-500,500" ) 
	(command "dimcontinue"  (zbth pdm3010 0 (cadr pdm301)) pdm306 ""  "")
	(command "dimlinear" pdm401 pdm4010 "h" "@-500,-500")
	(command "dimlinear" pdm401 pdm402 "v" "@-500,-500")
	(command "dimcontinue" (zbth pdm403 (car pdm401) 0 )""  "")
  (command "dimlinear"  (zbth pdm401 0 (cadr pdm403)) 		pdm403 "h" "@-500,500" ) 
	(command "dimcontinue" pdm404  pdm407 pdm408 (zbth pdm409 0 (cadr pdm408)) ""  "")
	
	(command "dimlinear" pdm501 pdm5010 "h" "@-500,-500")
	(command "dimlinear" pdm501 pdm502 "v" "@-500,-500")
	(command "dimcontinue" (zbth pdm503 (car pdm501) 0 )""  "")
  (command "dimlinear"  (zbth pdm501 0 (cadr pdm503)) 		pdm403 "h" "@-500,500" ) 
	(command "dimcontinue" pdm504  pdm507 pdm508 (zbth pdm509 0 (cadr pdm508)) ""  "")
	
	(command "dimlinear" pdm601 pdm602 "h" "@-500,-500")	
	(command "dimlinear" pdm606 pdm609 "v" "@1000,500")
	(command "dimlinear" pdm606 pdm607 "v" "@500,500")
	(command "dimcontinue" pdm608 pdm609 "" "") 
	
	(command "dimlinear" pdm701 pdm704 "h" "@-500,-500")	
	(command "dimlinear" pdm701 pdm702 "v" "@1000,500")
	
	
	
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm101 150 150) "" "n" "");tmd 用了四五天，就没明白，原来是 20，0 上面都得有引号。tmd，tdmd。指定内部点才行，真真他，，才明白。	bhatch,hatch,tmd qubie ?????
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm101 150 150) "" "n" "")
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm201 150 150) "" "n" "")
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm201 150 150) "" "n" "")
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm301 150 150) "" "n" "")
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm301 150 150) "" "n" "")
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm401 150 150) "" "n" "")
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm401 150 150) "" "n" "")
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm501 150 150) "" "n" "")
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm501 150 150) "" "n" "")
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm601 150 150) "" "n" "")
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm601 150 150) "" "n" "")
	(command "bhatch" "p"  "ansi31" "20" "0"  (zbjs pdm701 150 150) "" "n" "")
	(command "hatch" "p"  "ar-conc"  "5" "0"  (zbjs pdm701 150 150) "" "n" "")
)

(DEFUN zdm_gj	(/)
	(zdm_zbd)
	(command "CLAYER" "2"	);设当前图层为2
	
	(command "pline" pzd101 pzd102   pzd106 pzd107 pzd108 pzd109 pzd1010 pzd1011 pzd101 "")	
	(command "pline" pzd201 	pzd202 	 pzd2011 pzd2012 pzd2013 pzd2014 pzd2015 pzd2016 pzd201 "" )
	(command "pline" pzd301 	pzd302 	pzd3012	pzd306	pzd307	pzd308	pzd309	pzd3010 pzd3011 pzd301 "" )
	(command "pline" pzd401 	pzd402 	pzd407	pzd406	pzd405	pzd408	pzd409	 	pzd4010 	pzd4011		pzd4012 pzd4013 pzd401 "")	
	(setq ptx (+ 300 (* 300.00 (/ 500.00 (- h1 500)))));获齿墙对应底板上的点
  (gj_h1 pzd101 pzd102 pzd1011 (zbjs pzd1011 ptx h1))
	(gj_h2 (zbjs pzd1010 -600 0) (zbjs pzd109 600 0) pzd102 pzd106 )
	(gj_h1 pzd108 (zbjs pzd108 (* -1 ptx) h1) pzd107 pzd106)
	(setq ptx (+ 300 (* (/ 300.00 1100) 700) ));获齿墙对应底板上的点
	(gj_h1 pzd201 pzd202 pzd2016 (zbjs pzd2016 ptx 1600))
	(gj_h2 (zbjs pzd2015 -600 0) (zbjs pzd2014 600 0) pzd202 pzd2011)
	(gj_h1 pzd2013 (zbjs pzd2013 (* -1 ptx) h1) pzd2012 pzd2011)
	(setq pty (/ 500 (sin a3)))
	
	;inters tmd,no 想到，太多函数不熟，走了弯路。
	(setq linpd0 (inters pzd3011 pzd3010 pzd3012 pzd306 nil))
	(gj_h1  pzd301 pzd302 pzd3011 linpd0)
	;(command "line" pzd301 pzd302 "")
	;(command "LENGTHEN" "de" 1000 pzd302 "");len tmd 直线延长一定长度。又一个命令才想到。
	
	(setq gjx3012 (zbjs pzd3012 0 (- (/ ls (cos a3)))))
	(setq gjx306 (zbjs pzd306 (- hbaohu) (- (-  (/ ls (cos a3)) (* hbaohu (/ (sin a3) (cos a3)))))))
	(setq gjd3012 (zbjs pzd3012 (/ rgj 2) (/ (+ ls1 (* (/ rgj 2) (sin a3))) (cos a3) -1)));早应该用这个公式，一直不会化简，唉，三个月想一个问题。
	(setq gjd306 (zbjs pzd306 (- (+ hbaohu (/ rgj 2))) (- (-  (/ ls1 (cos a3)) (* (+ (/ rgj 2) hbaohu) (/ (sin a3) (cos a3)))))))
	
	(setq gjx3010 (zbjs pzd3010 0  (/ ls (cos a3))) )
	(setq linpd0 (inters pzd3010 pzd309 pzd307 pzd306 nil))
	(setq gjx309 (zbjs linpd0 (- hbaohu)  (+  (/ ls (cos a3)) (* hbaohu (/ (sin a3) (cos a3))))))
	(setq gjd3010 (zbjs pzd3010 (/ rgj 2) (/ (+ ls1 (* (/ rgj 2) (sin a3))) (cos a3))))
	(setq gjd309 (zbjs linpd0 (- (+ hbaohu (/ rgj 2)))  (+  (/ ls1 (cos a3)) (* (+ (/ rgj 2) hbaohu) (/ (sin a3) (cos a3))))))
	(command "line" gjx3012 gjx306 "")
	(command "line" gjx3010 gjx309 "")
  (setq gshu (+ (fix (+ (/ (abs (distance gjd3012 gjd306)) jjgj) 0.5)) 1)) ;钢筋根数gshu
  (setq jjgs1 (/ (abs (distance gjd3012 gjd306)) (- gshu 1)))	;应该实际调整间距。
	(setq lingjd3012 gjd3012)
	(setq lingjd3010 gjd3010)
	(setq pty (/ (distance gjd3012 gjd3010) 2))
	(setq n 0)
	(setq lina_gj1 (angle gjd3012 gjd306))
	(repeat  gshu 
		(setq n (1+ n))
		(command "donut" 0 rgj lingjd3012 "");draw right point 
		(command "donut" 0 rgj lingjd3010 "");draw left point 
		(if (> n (/ gshu 2)) 
			(command "pline" lingjd3012 "@-150,-300" lingjd3010 "") 
			(command "pline" lingjd3012 "@150,-300" lingjd3010 ""))
		
		(setq lingjd3012 (polar lingjd3012 lina_gj1 jjgs1))
		(setq lingjd3010 (polar lingjd3010 lina_gj1  jjgs1)	)
	);repeat finish
	
	(command "line" gjx3012 lingjx1-2  "")
	(command "LENGTHEN" "de" 600 gjx3010 "")
	
	(gj_h1 pzd308  (zbjs pzd308 ( * -1 1600 (/ 300   (- (cadr pzd309) (cadr pzd308)))) 1600) pzd307 pzd306);
	(command "LENGTHEN" "de" 600 lingjx1-2 "");锚固长度600
	(setq ptx (+ 300 (* (/ 300.00 1100) 500) ));获齿墙对应底板上的点
	(gj_h1 pzd401 pzd402 pzd4013 (zbjs pzd4013 ptx 1600))
	(gj_h2 (zbjs pzd4012 -600 0) (zbjs pzd4011 600 0) pzd402 pzd408)
	(gj_h1 pzd4010 (zbjs pzd4010 (* -1 ptx) 1600) pzd409 pzd408)
	
	(gj_h1 pzd407 pzd406 pzd408 pzd405)
	
	(command "line" lingjx1-2 lingjx2-2 "")
	(command "line" lingjx1-1 "@0,-500" "")
	(command "line" lingjx2-1 "@0,-500" "")
	
	(command "zoom" "a")
	(command "dimtxt" 200)		;标注文字高度2.5
	(command "DIMASZ" 100)		;箭头大小4-5
	(command "DIMDLI" 800)		;尺寸界线间距8
	(command "DIMEXE" 300)		; 尺寸界线超出量3-4
	(command "DIMEXO" 150)		;尺寸界线间隙,具体要看标注点的位置，比如，下面@后的数，要大于这个才会显示距离出来
	(command "DIMlfac" 0.1)		;尺寸测量比例
	(command "dimdec" 0);数据小数位 
	(command "dimdsep" ".");小数点应是。号，不是，
	(setq pty (cadr pzd409))
	(command "dimlinear" pzd409 pzd4010 "h" "@500,-500" "");
	(command "dimcontinue" pzd4011 pzd4012 pzd4013 pzd401 pzd308 (zbth pzd309 0 pty)  (zbth pzd3010 0 pty) (zbth pzd3011 0 pty) (zbth pzd301 0 pty)  (zbth pzd2013 0 pty) (zbth pzd2014 0 pty) (zbth pzd2015 0 pty) (zbth pzd2016 0 pty) (zbth pzd201 0 pty)  (zbth pzd108 0 pty) (zbth pzd109 0 pty) (zbth pzd1010 0 pty) (zbth pzd1011 0 pty) (zbth pzd10` 0 pty) "" ""  );
	(command "dimlinear" pzd409 pzd408 "v" "@500,-500" "");
	(command "dimcontinue" pzd405 pzd404 ""  "")
	(setq ptx (car pzd101))
	(command "dimlinear" pzd101 pzd102 "v" "@-500,-500" "");
	(command "dimlinear" pzd204 pzd205 "h" "@-500,500" "")
		(command "dimlinear" pzd303 pzd304 "h" "@-500,500" "")
	(command "dimlinear" pzd407 pzd406 "v" "@-500,500" "")
	(command "dimlinear" pzd406 pzd405 "h" "@-500,500" "")
		(command "dimlinear" pzd1011 pzd1010 "v" "@500,500" "")
	(command "dimlinear" pzd2016 pzd2015 "v" "@500,500" "")
		(command "dimlinear" pzd3011 pzd3010 "v" "@500,500" "")
	(command "dimlinear" pzd4013 pzd4012 "v" "@500,500" "")
		(command "dimlinear" pzd409 pzd405 "v" "@500,500" "")
	
	
)
(defun gj	(/)
	
	
	
	(command "CIRCLE" (polar (zd Pdb00 Pdb200) 0 15) 40);画编号 的圆，字体样式，字体宽0。7，50*0。7=35，
	;*****************溢流段
  (setq gjx5 (zbjs p26 ls hbaohu))	;(陡坡段第一条钢筋线的基点坐标
  (command "pline" gjx5 (zbjs p11 ls (- 0 (/ hbaohu (cos A)) (/ ls tga))) "" ) 	;绘第一条线钢筋。
  (setq gjd5 (zbjs p26 ls1 (+ hbaohu (/ rgj 2))))
  (setq gjd51 (zbjs 		p11 		ls1  	(*	-1		(+ (/ 500 (cos A)) (/ ls1 (cos A)) (/ ls1 tga))	)		)  )		
  (setq gjx6 (zbjs p27 (* -1 ls) hbaohu)) ;(陡坡段第二条竖钢筋线的基点坐标
  (command "pline"  gjx6 (zbjs  (zbjs p11 500 (/ -500 tga))
													 (* -1 ls)  (* -1 (- (/ ls (cos A)) (/ ls tga))) )
		"" ) 	;把点钢筋这一侧的点坐标用P11算出来，
  (setq gjd6 (zbjs p27 (* -1 ls1) (+ hbaohu (/ rgj 2))))
  (setq gjd61 (zbjs 			p28 		(* -1 ls1)		(- 0 (/ (- hbaohu (/ rgj 2)) (cos A)) (/ ls1 tga))	)) 	;点钢筋计到板下，板中不计，设腰筋。
  
  (gshujs gjd5 gjd51)
  (setq jid00 gjd5)
  (setq pdb5 (polar (zd gjd5 gjd6) (* 1.5 pi) 120))
  (setq pdb pdb5)
  (setq ptx 0)
  (setq pty jjgj)
  (huagjd1 (* 1.5 pi))
  (setq pdb51 (zbjs pdb 0 -200))；
	
  (gshujs gjd6 gjd61)
  (setq jid00 gjd6)
  (setq pdb pdb5)
  (setq ptx 0)
  (setq pty jjgj)
  (huagjd1 (* 1.5 pi))
  (command "line" pdb5 pdb51 "")
  (command "line" pdb5 "@1000,0" "")	;
  (command "text" "j" "mL" (zd Pdb00 Pdb200) 50 0 "31  %%130 8 @200");31号筋
	(command "CIRCLE" (polar (zd Pdb00 Pdb200) 0 15) 40);画编号 的圆，字体样式，字体宽0。7，50*0。7=35，
  (setq gjx7 (zbjs p30 ls hbaohu))	;(第一条钢筋线的基点坐标
  (command "pline" gjx7 (zbjs (zbjs p12 -500 (/ 500 tga))
													ls (- 0 (/ hbaohu (cos A)) (/ ls tga))
												) "" ) 	;绘第一条线钢筋。
  (setq gjd7 (zbjs p30 ls1 (+ hbaohu (/ rgj 2))))
  (setq gjd71 (zbjs p29 ls1 (* -1 (+ (/ ls1 (cos A)) (/ ls1 tga)))) )
  (setq gjx8 (zbjs p31 (* -1 ls) hbaohu)) ;(第一条钢筋线的基点坐标
  (command "pline" gjx8 (zbjs p12 (* -1 ls) (- (/ ls (cos A)) (/ ls tga))) "" ) 	;
  (setq gjd8 (zbjs p31 (* -1 ls1) (+ hbaohu (/ rgj 2))))
  (setq gjd81 (zbjs 		p12 		(* -1 ls1)
								(- (/ ls1 tga) (/ ls1 (cos A)) (/ 500 (cos A)))
							) ) 	;
	
  (gshujs gjd7 gjd71)
  (setq jid00 gjd7)
  (setq pdb7 (polar (zd gjd7 gjd8) (* 1.5 pi) 120))
  (setq pdb pdb7)
  (setq ptx 0)
  (setq pty jjgj)
  (huagjd1 (* 1.5 pi))
  (setq pdb71 (zbjs pdb 0 -200))
  (setq jjgj 200)
  (gshujs gjd8 gjd81)
  (setq jid00 gjd8)
  (setq pdb pdb7)
  (setq ptx 0)
  (setq pty jjgj)
  (huagjd1 (* 1.5 pi))
  (command "line" pdb7 pdb71 "")
  (command "line" pdb7 "@0,-200" "@500,0" "")	;
	(gj_bh 31 8)
  
  (command "line" (zd gjd1 gjd11) (zd gjd4 gjd41) "")
	(command "text" "j" "mL" (zd (zd gjd1 gjd11) (zd gjd4 gjd41)) 50 0 "22  %%130 8 @200");22号筋
	(command "CIRCLE" (polar (zd (zd gjd1 gjd11) (zd gjd4 gjd41)) 0 15) 40);画编号 的圆，字体样式，字体宽0。7，50*0。7=35，
  
  (command "line" (zbjs gjx5 0 (+ jjgj hbaohu)) "@280,0"
		"@0,-1500" "@1000,0" "" ) 	;标注5的线钢筋
  (gj_bh 29 12)
	
  (command "line"
		(zbjs gjx6 0 (+ jjgj hbaohu)) "@200,0" "@0,-800" "@200,0" ""
  ) 	;标注6的线钢筋，此处的800，是为了避免和上个1000相重合，可以调整的
  (gj_bh 30 12)
	
  (command "line" (zbjs gjx7 0 (+ jjgj hbaohu)) "@-280,0" "@0,-800" "@-1000,0" "" ) 	;标注7的线钢筋
  (gj_bh 32 12)
  (command
		"line" (zbjs gjx8 0 (+ jjgj hbaohu)) "@-280,0" "@0,-1500"
		"@200,0" "" ) 	;标注8的线钢筋
	(gj_bh 33 12)
	
	
  (setq gjx1 (zbjs p31 ls hbaohu))	;(第一条钢筋线的基点坐标
  (command
		"pline"
		gjx1
		(polar gjx1 (/ pi 2) (- 1500 (* 2 hbaohu)))
		""
  ) 	;***消力池***绘第一条线钢筋。
  (setq gjd1 (zbjs p31 ls1 (+ hbaohu (/ rgj 2))))
  (setq
		gjd11 (zbjs p12 ls1 (* -1 (+ hbaohu (/ rgj 2))))
  ) 	;另一条竖钢筋2
  (setq gjx2 (zbjs p32 (* -1 ls) hbaohu)) ;(第一条钢筋线的基点坐标
  (command
		"pline"
		gjx2
		(polar gjx2 (/ pi 2) (- 1500 (* 2 hbaohu)))
		""
  ) 	;
  (setq gjd2 (zbjs p32 (* -1 ls1) (+ hbaohu (/ rgj 2))))
  (setq gjd21 (zbjs p33 (* -1 ls1) (- hbaohu (/ rgj 2))))
	;点钢筋计到板下，板中不计，设腰筋。
	
  (gshujs gjd2 gjd21)
  (setq jid00 gjd1)
  (setq jid000 gjd2)
  (setq pdb00 (polar (zd jid00 jid000) (* 1.5 pi) 120))
  (setq ptx 0)
  (setq pty jjgj)
  (huagjd (* 1.5 pi))			;画钢筋点
  (setq pdb01 Pdb)
  (command "line" pdb00 pdb01 "")	;***3
  (setq gjx3 (zbjs p35 ls hbaohu))	;(第一条钢筋线的基点坐标
  (command
		"pline"
		gjx3
		(polar gjx3 (/ pi 2) (- 1500 (* 2 hbaohu)))
		""
  ) 	;绘第一条线钢筋。
  (setq gjd3 (zbjs p35 ls1 (+ hbaohu (/ rgj 2))))
  (setq
		gjd31 (zbjs p34 ls1 (* -1 (+ hbaohu (/ rgj 2))))
  ) 	;***4
  (setq gjx4 (zbjs p36 (* -1 ls) hbaohu)) ;(第一条钢筋线的基点坐标
  (command
		"pline"
		gjx4
		(polar gjx4 (/ pi 2) (- 1500 (* 2 hbaohu)))
		""
  ) 	;
  (setq gjd4 (zbjs p36 (* -1 ls1) (+ hbaohu (/ rgj 2))))
  (setq
		gjd41 (zbjs p36 (* -1 ls1) (- 1500 hbaohu (/ rgj 2)))
  ) 	;点钢筋计到板下，板中不计，设腰筋。
  (gshujs gjd3 gjd31)
  (setq jid00 gjd3)
  (setq jid000 gjd4)
  (setq pdb200 (polar (zd gjd3 gjd4) (* 1.5 pi) 120))
  (setq ptx 0)
  (setq pty jjgj)
  (huagjd (* 1.5 pi))
  (setq pdb01 Pdb)
  (command "line" pdb00 pdb200 "")
  (command "line" pdb200 Pdb "")	;
	(command "text" "j" "mL" (zd Pdb00 Pdb200) 50 0 "40  %%130 8 @200");40号筋
	(command "CIRCLE" (polar (zd Pdb00 Pdb200) 0 15) 40);画编号 的圆，字体样式，字体宽0。7，50*0。7=35，
  
  (command "line" (zd gjd1 gjd11) (zd gjd4 gjd41) "")
  (command "text" "j" "mL" (zd (zd gjd1 gjd11) (zd gjd4 gjd41)) 50 0 "39  %%130 8 @200");*****39******号筋
	(command "CIRCLE" (polar (zd (zd gjd1 gjd11) (zd gjd4 gjd41)) 0 15) 40);画编号 的圆，字体样式，字体宽0。7，50*0。7=35，
	;************************************;***************************绘横钢筋
	;***************
  (setq gjx1 (zbjs jidian hbaohu (* -1 ls))) ;(第一条钢筋线的基点坐标
  (command
		"pline"
		gjx1
		(polar gjx1 0 (- l1 (* 2 hbaohu)))
		""
  ) 	;绘第一条横钢筋。
  (setq
		gjd1 (zbjs jidian (+ hbaohu (/ rgj 2)) (* -1 ls1))
  )
  (setq
		gjd11 (zbjs p11 (* -1 (+ hbaohu (/ rgj 2))) (* -1 ls1))
  ) 	;另一条2
  (setq gjx2 (zbjs gjx1 0 (* -1 (- houdbgj ls ls))))
	;本次利用对称 计算，换一种方法。
  (command
		"pline"
		gjx2
		(polar gjx2 0 (- l1 (* 2 hbaohu)))
		""
  );上面gjx1是上横线，gjx2是下面的第二条横线。对应。
  (setq gjd2 (zbjs gjd1 0 (* -1 (- houdbgj ls1 ls1))))
  (setq gjd21 (zbjs gjd11 0 (* -1 (- houdbgj ls1 ls1)))) ;
  (gshujs gjd2 gjd21)
  (setq jid00 gjd1)
  (setq jid000 gjd2)
  (setq pdb00 (polar (zd jid00 jid000) (* -1 pi) 120))
  (setq ptx jjgj)
  (setq pty 0)
  (huagjd (* 1 pi))			;画钢筋点
  (command "line" pdb pdb00 "")
  (command "line" pdb00 "@0,1000" "@200,0" "")
	(gj_bh 20 8)
	
  (command "line"  (polar gjx2 0 1000) "@0,1000"  "@500,0" "") ;**
	(gj_bh 21 8)
	
  (setq Pd0 (polar p36 (* pi 0.5) 1500)) ;向上，270度求临时点
  (xgjjs p12 pd0 3)
  (setq jid3 pt1)			;(第一条钢筋线的基点坐标
  (setq jid4 pt2)
  (command "pline" jid3 jid4 "")	;绘消力池底板第一条横钢筋。
  (setq jid03 pt01)
  (setq jid04 pt02)
  (gsjsh l3 hbaohu 200 jid03)		;gsjsh阵列画点
  (setq pdb01 jid03)			;传递点坐标，下次调用。;横钢筋2
  (setq Pd1 (polar pd0 (* pi 1.5) houdbgj)) ;利用上一个点求
  (setq Pd0 (polar p12 (* pi 1.5) houdbgj)) ;向下，270度求临时点
  (xgjjs pd0 pd1 2)
  (setq jid3 pt1)			;(第二条钢筋线止点钢筋的基点坐标
  (setq jid4 pt2)
  (command "pline" jid3 jid4 "")	;绘第二条横钢筋。
  (setq jid03 pt01)
  (setq jid04 pt02)
  (gsjsh l3 hbaohu 200 jid03)		;第二条横钢筋的钢筋点阵列画点。
  (setq lls (bz1 p35 p36))		;******以下为画标注钢筋的线22，*****
  (setq Pdb (zbjs jid03 (* 1 lls) (* 1 lls)))
  (command "line" jid03 Pdb "")
  (command "array" "l" "" "r" 1 gshu jjgs1)
  (setq Pdb0 (zbjs jid04 (* 1 lls) (* 1 lls)))
  (command "line" pdb Pdb0 "@1000,0" "")
  (command "line" pdb01 Pdb "")
  (command "array" "l" "" "r" 1 gshu jjgs1);阵列多个点，
  
  (setq pdz (zbjs pdb0 500 0))
	
	(gj_bh 37 8)
	;;****绘斜钢筋线
	;********************
  (setq
		gjx11 (zbjs p11 ls (* -1 (+ (/ ls (cos A)) (* ls tga))))
  )
  (setq
		gjx12 (zbjs						p12						(* -1 ls)						(* -1 (- (/ ls (cos A)) (* ls tga)))					))
  (command "pline" gjx11 gjx12 "")	;绘斜钢筋线。
  (setq ptx ls1)			;计算点钢筋坐标
  (setq pty (* -1 (+ (/ ls1 (cos A)) (* ls1 tga))))
  (setq gjd11 (zbjs p11 ptx pty))	;做程序的时候用上一点，gjx11，是错了现在改了
  (setq ptx (* -1 ls1))
  (setq pty (* -1 (- (/ ls1 (cos A)) (* ls1 tga))))
  (setq gjd12 (zbjs p12 ptx pty))
  (command "donut" 0 rgj gjd11 "")	;此处画上面的钢筋点。
  (setq pyls (/ (- houdbgj ls ls) (cos A))) ;绘第二条斜钢筋
  (setq gjx13 (polar gjx11 (* pi 1.5) pyls))
  (setq gjx14 (polar gjx12 (* pi 1.5) pyls))
  (command "pline" gjx13 gjx14 "")	;绘斜钢筋线。
  (setq ptls (/ (- houdbgj ls1 ls1) (cos A))) ;
  (setq gjd13 (polar gjd11 (* pi 1.5) ptls))
  (setq gjd14 (polar gjd12 (* pi 1.5) ptls))
  (command "donut" 0 rgj gjd13 "")
  (gshujs gjd11 gjd12)
  (setq px1 (* jjgj (sin A)))
  (setq py1 (* -1 (* jjgj (cos A))))
  (command "donut" 0 rgj gjd11 "")
  (setq jid00 gjd11)			;循环画点。
  (setq jid000 gjd13)			;循环画点。
  (setq jshu gshu)
  (repeat
		(- gshu (fix (/ gshu 2)))
		(command "donut" 0 rgj jid00 "")
		(command "donut" 0 rgj jid000 "")
		(setq pdb (polar (zd jid00 jid000) (- (* 2 pi) A) 120))
		;直接输入在中线上的长120，下一个是80，合起来是200间距，
		(command "line" jid00 pdb "")
		(command "line" jid000 pdb "")
		(setq jid00 (zbjs jid00 px1 py1))
		(setq jid000 (zbjs jid000 px1 py1))
  )
  (setq bzd011 (polar pdb A 1000))
  (command "line" pdb bzd011 "@1000,0" "")
	(gj_bh 27 12)
  
  (repeat
		(- (fix (/ gshu 2)) 0)
		(command "donut" 0 rgj jid00 "")
		(command "donut" 0 rgj jid000 "")
		(setq pdb (polar (zd jid00 jid000) (- pi A) 80)) ;
		(command "line" jid00 pdb "")
		(command "line" jid000 pdb "")
		(setq jid00 (zbjs jid00 px1 py1))
		(setq jid000 (zbjs jid000 px1 py1))
  )
  (command
		"line"
		(polar (zd gjd11 gjd13) (- (* 2 pi) A) 120)
		(polar (zd gjd12 gjd14) (- pi A) 80)
		""
  ) 	;连接点。
  (command "line" (polar gjx13 (- (* 2 pi) A) (+ jjgj hbaohu))
		(polar (polar gjx13 (- (* 2 pi) A) (+ jjgj hbaohu))
			A 1500 ) "@1000,0" ""
  ) 	
  (gj_bh 40 8)
	
  ;***************消力池消力坎钢筋;此有锚固长度的要求，;****************消力池出口侧1
  (setq ptx 0)
  (setq pty (* -1 500))
  (setq linPd0 (zbjs p13 ptx pty))		;用P1３求临时点
  (xgjjs linpd0 p14 0)
  (setq gjx1-1 pt1)			;(第一条钢筋线的基点坐标
  (setq gjx1-2 pt2)
  (command "pline" gjx1-1 gjx1-2 "")	;绘第一条线钢筋。
  (setq gjd1-1 pt01)
  (setq gjd1-2 pt02)
	
  (setq linpd15 (polar p15 (/ pi -2) 1000) )	;**;** 2
  (xgjjs linpd15 p15 1)
  (setq gjx2-1 pt1)			;(第2条钢筋线的基点坐标
  (setq gjx2-2 pt2)
  (command "pline" gjx2-1 gjx2-2 "")	;绘线钢筋。
  (setq gjd2-1 pt01)
  (setq gjd2-2 pt02)
	(gshujs gjd1-1 gjd1-2)
	(command "donut" 0 rgj gjd1-1 "");线1上的点 tmd donut 不是donut
	(command "array" "l" "" "r" gshu 1 jjgs1)
	(command "donut" 0 rgj gjd2-1 "");线2上的点
	(command "array" "l" "" "r" gshu 1 jjgs1)
	(command "pline" gjd1-1 (polar (zd gjd1-1 gjd2-1) (/ pi 2) 200) gjd2-1 "");画指向点钢筋的线。折线，然后镜像。
	(command "array" "l" "" "r" gshu 1 jjgs1)
	(command "pline"  (polar (zd gjd1-1 gjd2-1) (/ pi 2) 200) (polar (zd gjd1-2 gjd2-2) (/ pi 2) 200) "@0,500" "@500,0" "")
	(setq linpm01 (getvar "lastpoint"))
	(gj_bh 42 8)
	(command "pline" (polar gjx1-2 (/ pi -2) 200) "@1000,0" "")
	(gj_bh 41 12) 
	
)

(defun hdm_gj()
	(command "clayer" "2")
	(command "rectang" "47000,34700" "@42000,29700" );A3 on 1:100 print,truse 
	(command "CLAYER" "3"	"lweight" "bylayer");设当前图层为3,线宽随层
	(command "rectang" "49500,35700" "@38500,27700" );add 25mm,10mm+297+50
	(command "clayer" "2")
	(setq jidian (list 49500 35700));图框的角点
	(hdmfuzi)
	(command "clayer" "2")
	(command "rectang" "47000,0" "@42000,29700" );A3 on 1:100 print,truse 
	(command "CLAYER" "3"	"lweight" "bylayer");设当前图层为3,线宽随层
	(command "rectang" "49500,1000" "@38500,27700" )
	
	
	(command "clayer" "2")
	
	(command "pline" pdm101 pdm105 pdm106 pdm107 pdm108  pdm109 pdm101 "")
	(command "pline" pdm201 pdm202  pdm203 pdm204 pdm205 pdm206 pdm207 pdm208 pdm209 pdm2010  pdm201 "")
	
	
	(command "pline" pdm301 pdm302 pdm303 pdm304 pdm305  pdm306  pdm301   "")
	(command "pline" pdm302 pdm305   "") 
	(command "pline" pdm307 pdm308 pdm309  pdm3010  pdm307      "")
	
	(command "pline" pdm401 	pdm402	pdm403	pdm404	pdm405	pdm406	pdm407	pdm408	pdm409 pdm4010 pdm401 "")
	(command "pline" pdm501 	pdm502	pdm503	pdm504	pdm505	pdm506	pdm507	pdm508	pdm509	pdm5010 pdm501  "")
	(command "pline" pdm601 	pdm605	pdm606	pdm607	pdm608	pdm609	pdm601 "")
	(command "pline" pdm701 pdm702 pdm703 pdm704 pdm701 "")
	(command "zoom" "a")
	
	(gj_h1 pzd101 pzd102 pzd109 pzd108)
	(gj_h1 pzd102 pzd103 pzd108 pzd107)
	(gj_h1 pzd102 pzd103 pzd108 pzd107)
	(gj_h1 pzd103 pzd105 pzd107 pzd106)
	
	(gj_h1 pzd202 pzd203 pzd205 pzd204)
	(command "line" gjx1-1 "@0,-500" "")
	(command "line" gjx2-1 "@0,-500" "")
	(gj_h1 pzd201 pzd2020 pzd202 pzd209)
	(gj_h1 pzd206 pzd207 pzd209 pzd208)
	
	(gj_h1 pzd301 pzd302 (zbth pzd301 (car pzd307) 0)  (zbth pzd306 (car pzd3010) 0))
	(gj_h1 (zbth pzd306 (car pzd3010) 0)  (zbth pzd305 (car pzd309) 0) pzd306 pzd305)
	
	(gj_h2  pzd301 pzd306 (zbth pzd301  0 (cadr pzd307))  (zbth pzd306  0 (cadr pzd3010)))
	(gj_h2 (zbth pzd302  0 (cadr pzd308))  (zbth pzd305  0 (cadr pzd309)) pzd302 pzd305)
	
	(gj_h1 pzd402 pzd403 pzd405 pzd404)
	(command "line" gjx1-1 "@0,-500" "")
	(command "line" gjx2-1 "@0,-500" "")
	(gj_h1 pzd401 pzd4010 pzd402 pzd409)
	(gj_h1 pzd406 pzd407 pzd409 pzd408)
	
	(gj_h1 pzd502 pzd503 pzd505 pzd504)
	(command "line" gjx1-1 "@0,-500" "")
	(command "line" gjx2-1 "@0,-500" "")
	(gj_h1 pzd501 pzd5010 pzd502 pzd509)
	(gj_h1 pzd506 pzd507 pzd509 pzd508)
	
	(gj_h1 pzd601 pzd102 pzd609 pzd108)
	(gj_h1 pzd602 pzd603 pzd608 pzd607)
	(gj_h1 pzd602 pzd603 pzd608 pzd607)
	(gj_h1 pzd603 pzd605 pzd607 pzd606)
	
	(gj_h1 pzd701 pzd702 pzd704 pzd703)
	
	(command "pline" pzd203 pzd204 pzd205 pzd206 pzd207 pzd208 pzd209 pzd2010 pzd203 "")
	(gj_h1 pzd203 pzd204 (zbjs pzd203 250 0) pzd204)
	(gj_h1 (zbjs pzd2010 -250 0) pzd208 pzd2010 pzd209)
	(gj_h2 pzd203 pzd2010 (zbjs pzd206 -250 0) (zbjs pzd207 250 0))
	
	;开始标注，因为桥面板在前，先标他
	
	(command "clayer" "2")
	;桥面板标注
	(command "dimlinear" pzd203 pzd2010 "h" "@-500,-500" "")
	(command "dimlinear" pzd203 pzd205 "v" "@-500,500" "")
	
	(command "dimcontinue" pzd208  pzd209 "" "") 
	
	(command "clayer" "2")
	(command "dimlinear" pdm101 pdm102 "h" "@-500,-500")	
	(command "dimlinear" pdm106 pdm109 "v" "@1000,500")
	(command "dimlinear" pdm106 pdm107 "v" "@500,500")
	(command "dimcontinue" pdm108 pdm109 "" "") 
	
	(command "dimlinear" pdm201 pdm2010 "h" "@-500,-500")
	(command "dimlinear" pdm201 pdm202 "v" "@-500,-500")
	(command "dimcontinue" (zbth pdm203 (car pdm201) 0 )""  "")
	;桥面板标注
  (command "dimlinear"  (zbth pdm201 0 (cadr pdm203)) 		pdm203 "h" "@-500,500" ) 
	(command "dimcontinue" pdm204 (zbth pdm20z 0 (cadr pdm203) )""  "")
	(command "dimlinear"  (zbth pdm20z 0 (cadr pdm207) ) 	pdm207 "h" "@-500,500" ) 
	(command "dimcontinue" pdm208 (zbth pdm209  0 (cadr pdm203) )""  "")
	(command "dimlinear" pdm2010	pdm209 "v" "@500,500" ) 
	(command "dimcontinue" (zbth pdm208  (car pdm209)  0 ) ""  "")
	;桥面板标注
	
	(command "dimlinear"  pdm301 pdm302 "v" "@-500,500" ) 
	(command "dimcontinue"  pdm303 ""  "")
	(command "dimlinear"  pdm301 pdm303 "v" "@-1000,500" ) 
	(command "dimlinear"  pdm301 (zbth pdm307 0 (cadr pdm301)) "h" "@-500,500" ) 
	(command "dimcontinue"  (zbth pdm3010 0 (cadr pdm301)) pdm306 ""  "")
	(command "dimlinear" pdm401 pdm4010 "h" "@-500,-500")
	(command "dimlinear" pdm401 pdm402 "v" "@-500,-500")
	(command "dimcontinue" (zbth pdm403 (car pdm401) 0 )""  "")
  (command "dimlinear"  (zbth pdm401 0 (cadr pdm403)) 		pdm403 "h" "@-500,500" ) 
	(command "dimcontinue" pdm404  pdm407 pdm408 (zbth pdm409 0 (cadr pdm408)) ""  "")
	
	(command "dimlinear" pdm501 pdm5010 "h" "@-500,-500")
	(command "dimlinear" pdm501 pdm502 "v" "@-500,-500")
	(command "dimcontinue" (zbth pdm503 (car pdm501) 0 )""  "")
  (command "dimlinear"  (zbth pdm501 0 (cadr pdm503)) 		pdm403 "h" "@-500,500" ) 
	(command "dimcontinue" pdm504  pdm507 pdm508 (zbth pdm509 0 (cadr pdm508)) ""  "")
	
	(command "dimlinear" pdm601 pdm602 "h" "@-500,-500")	
	(command "dimlinear" pdm606 pdm609 "v" "@1000,500")
	(command "dimlinear" pdm606 pdm607 "v" "@500,500")
	(command "dimcontinue" pdm608 pdm609 "" "") 
	
	(command "dimlinear" pdm701 pdm704 "h" "@-500,-500")	
	(command "dimlinear" pdm701 pdm702 "v" "@1000,500")
	
	
	
	
	;桥面板
	(gj_h1 (polar p40 0 -250) p38 p40 p39)
	(command "pline" lingjx1-2 lingjx2-2 "")
	(command "pline" (zd lingjx1-1 lingjx1-2) "@-500,0" "")
	(gj_bh 12 12);steel 7
	(command "pline" (zd lingjdzd0 lingjdzd) "@800,0" "")
	(gj_bh 13 8);9 号筋 steel
	(xgjjs p37 p38 0)
	(command "pline" lingjx1-1  pt1 "")
	(command "pline" lingjx2-1 (polar pt1 0 250) "");向下延伸 得钢筋线的一点。
	(gj_h1 p41 p42 (polar p41 0 250) p43)
	(command "pline" lingjx1-2 lingjx2-2 "")
	(command "pline" (zd lingjx2-1 lingjx2-2) "@500,0" "")
	(gj_bh 12 12);steel 7
	(command "pline" (zd lingjdzd0 lingjdzd) "@800,0" "")
	(gj_bh 13 8);9 号筋 steel
	(xgjjs p44 p43 0)
	(command "pline" lingjx2-1  pt1 "")
	(command "pline" lingjx1-1 (polar pt1 0 -250) "");向下延伸 得钢筋线的一点。
	
	(gj_h2 p37 p44 (polar p40 0 -250) (polar p41 0 250));
	(setq linpd0 (getvar "lastpoint"))
	(command "pline" linpd0 "@800,0" "")
	(gj_bh 15 8)
	(command "pline"  (zd lingjx2-1 lingjx2-2) "@0,-800" "@500,0" "")
	(gj_bh 15 12)
	
	(xgjjs (polar p40 0 -250) (polar p41 0 250) 3);桥面板主筋配筋，
	(command "pline"  pt1 pt2 "")
	(setq linpd0 (zbjs pt01 150 150))
	(command "donut" 0 rgj pt01 "")
	(command "array" "l" "" "r" 1 gshu  jjgs1)
	(command "pline" pt01 linpd0 "")
	(command "array" "l" "" "r" 1 gshu jjgs1)
	(command "pline" linpd0 (zbjs linpd0 b 0) "@500,0" "")
	(gj_bh 14 8)
	(command "pline" pt1 "@500,0" "@0,500"  "@500,0" "")
	(gj_bh 15 12);;;;
	
	(gshujs (polar  pdm4012 0 -300) (polar pdm4013 0 300) );主筋，桥面板下面。
	(xgjjs (polar  pdm4012 0 -300) (polar pdm4013 0 300)  4)
	(command "pline"  pt1 pt2 "")
	(setq linpd0 (zbjs pt01 150 -150))
	(command "donut" 0 rgj pt01 "")
	(command "array" "l" "" "r" 1 gshu jjgs1)
	(command "pline" pt01 linpd0 "")
	(command "array" "l" "" "r" 1 gshu jjgs1)
	(command "pline" linpd0 (zbjs linpd0 b 0) "@500,0" "")
	(gj_bh 16 8)
	(command "pline" (zd pt1 pt2) "@0,-500" "")
	(gj_bh 17 18)
	;	命令: donut  
	;指定圆环的内径 <0.0000>:
	;指定圆环的外径 <55.0000>:
	;指定圆环的中心点或 <退出>:
	;	
)

(defun gjbfuzi ()
	(biaoz)
	
	
	(setq  	  gjbbh 1 	  gjbxs 950  gjbzj 8 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	;
	(setq gjbfuzi1  (list (list  gjbbh (nth 0 gjb_list1 )) 	(list  gjbxs (nth 1 gjb_list1 ))	(list  gjbzj (nth 2 gjb_list1 ) )	(list  gjbjc (nth 3 gjb_list1 ) )	(list  gjbgc (nth 4 gjb_list1 ) ) 	(list  gjbdgc (nth 5 gjb_list1 ))	(list  gjbgs  (nth 6 gjb_list1 ))	(list  gjbzc (nth 7 gjb_list1 ) )	(list  gjbdwz (nth 8 gjb_list1 ) )	(list  gjbzl (nth 9 gjb_list1 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 2 	  gjbxs (- l1 1000 50) 	 gjbxs1 (- 1000 50) gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi2  (list (list  gjbbh (nth 0 gjb_list2 )) 	(list  gjbxs (nth 1 gjb_list2 ))	(list  gjbxs1 (nth 2 gjb_list2 )) (list  gjbzj (nth 3 gjb_list2 ) )	(list  gjbjc (nth 4 gjb_list2 ) )	(list  gjbgc (nth 5 gjb_list2 ) ) 	(list  gjbdgc (nth 6 gjb_list2 ))	(list  gjbgs  (nth 7 gjb_list2 ))	(list  gjbzc (nth 8 gjb_list2 ) )	(list  gjbdwz (nth 9 gjb_list2 ) )	(list  gjbzl (nth 10 gjb_list2 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 3 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi3  (list (list  gjbbh (nth 0 gjb_list3 )) 	(list  gjbxs (nth 1 gjb_list3 ))	(list  gjbzj (nth 2 gjb_list3 ) )	(list  gjbjc (nth 3 gjb_list3 ) )	(list  gjbgc (nth 4 gjb_list3 ) ) 	(list  gjbdgc (nth 5 gjb_list3 ))	(list  gjbgs  (nth 6 gjb_list3 ))	(list  gjbzc (nth 7 gjb_list3 ) )	(list  gjbdwz (nth 8 gjb_list3 ) )	(list  gjbzl (nth 9 gjb_list3 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 4 	  gjbxs (- l1 50) 	gjbxs1 (- l1 50)  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi4  (list (list  gjbbh (nth 0 gjb_list4 )) 	(list  gjbxs (nth 1 gjb_list4 ))	(list  gjbxs1 (nth 2 gjb_list4 )) (list  gjbzj (nth 3 gjb_list4 ) )	(list  gjbjc (nth 4 gjb_list4 ) )	(list  gjbgc (nth 5 gjb_list4 ) ) 	(list  gjbdgc (nth 6 gjb_list4 ))	(list  gjbgs  (nth 7 gjb_list4 ))	(list  gjbzc (nth 8 gjb_list4 ) )	(list  gjbdwz (nth 9 gjb_list4 ) )	(list  gjbzl (nth 10 gjb_list4 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 5 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi5  (list (list  gjbbh (nth 0 gjb_list5 )) 	(list  gjbxs (nth 1 gjb_list5 ))	(list  gjbzj (nth 2 gjb_list5 ) )	(list  gjbjc (nth 3 gjb_list5 ) )	(list  gjbgc (nth 4 gjb_list5 ) ) 	(list  gjbdgc (nth 5 gjb_list5 ))	(list  gjbgs  (nth 6 gjb_list5 ))	(list  gjbzc (nth 7 gjb_list5 ) )	(list  gjbdwz (nth 8 gjb_list5 ) )	(list  gjbzl (nth 9 gjb_list5 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 6 	  gjbxs 500  	  gjbgc 0  gjbjc gjbxs gjbdgc (+ gjbjc gjbgc)  gjbxs1 (- l1 500) 	  gjbzj 12 	  gjbjc1 gjbxs1 	  gjbgc 0 	  gjbdgc1 (+ gjbjc1 gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs (+ gjbdgc1 gjbdgc)) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位bh 编号 xs 型式 gc 钩长 zj 直径  jc 净长 dgc 单根长 gs 根数 zc 总长 dwz 单位重 zl 总量
	
	(setq gjbfuzi6  (list (list  gjbbh (nth 0 gjb_list6 )) 	(list  gjbxs (nth 1 gjb_list6 ))	(list  gjbjc (nth 2 gjb_list6 ) )	(list  gjbgc (nth 3 gjb_list6 ) )	(list  gjbdgc (nth 4 gjb_list6 ) ) 	(list  gjbxs1 (nth 5 gjb_list6 ))	(list  gjbzj  (nth 6 gjb_list6 ))	(list  gjbjc1 (nth 7 gjb_list6 ) )	(list  gjbgc (nth 8 gjb_list6 ) )	(list  gjbdgc1 (nth 9 gjb_list6 ) )  (list  gjbgs  (nth 10 gjb_list6 ))	(list  gjbzc (nth 11 gjb_list6 ) )	(list  gjbdwz (nth 12 gjb_list6 ) )	(list  gjbzl (nth 13 gjb_list6 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 7 	  gjbxs11  500 gjbxs12  950 gjbxs21  1900 gjbxs22  950 	  gjbzj 12 	  gjbjc (+ gjbxs21 gjbxs22)  gjbjc1 (+ gjbxs11 gjbxs12) 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc)  gjbdgc1 (+ gjbjc1 gjbgc)	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs (+ gjbdgc1 gjbdgc)) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2) )   ;先是下层，然后是上层，***********此处再算？？？？？？？？？？？？？？？？？？？？
	
	(setq gjbfuzi7  (list (list  gjbbh (nth 0 gjb_list7 )) 	(list  gjbxs22 (nth 1 gjb_list7 ))	(list  gjbzj (nth 2 gjb_list7 ) )	(list  gjbjc (nth 3 gjb_list7 ) )	(list  gjbgc (nth 4 gjb_list7 ) ) 	(list  gjbdgc (nth 5 gjb_list7 ))	(list  gjbgs  (nth 6 gjb_list7 ))	(list  gjbzc (nth 7 gjb_list7 ) )	(list  gjbdwz (nth 8 gjb_list7 ) )	(list  gjbzl (nth 9 gjb_list7 ) )  			(list  gjbxs11 (nth 10 gjb_list7 ) ) (list  gjbgc (nth 11 gjb_list7 ) )  (list gjbjc1  (nth 12 gjb_list7 ) ) (list gjbdgc1 (nth 13 gjb_list7 ) ) (list  gjbxs12 (nth 14 gjb_list7 ) ) (list  gjbxs21 (nth 15 gjb_list7 ) ) )
	);line fuzi finish
	
	(setq  	  gjbbh 8 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi8  (list (list  gjbbh (nth 0 gjb_list8 )) 	(list  gjbxs (nth 1 gjb_list8 ))	(list  gjbzj (nth 2 gjb_list8 ) )	(list  gjbjc (nth 3 gjb_list8 ) )	(list  gjbgc (nth 4 gjb_list8 ) ) 	(list  gjbdgc (nth 5 gjb_list8 ))	(list  gjbgs  (nth 6 gjb_list8 ))	(list  gjbzc (nth 7 gjb_list8 ) )	(list  gjbdwz (nth 8 gjb_list8 ) )	(list  gjbzl (nth 9 gjb_list8 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 9 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi9  (list (list  gjbbh (nth 0 gjb_list9 )) 	(list  gjbxs (nth 1 gjb_list9 ))	(list  gjbzj (nth 2 gjb_list9 ) )	(list  gjbjc (nth 3 gjb_list9 ) )	(list  gjbgc (nth 4 gjb_list9 ) ) 	(list  gjbdgc (nth 5 gjb_list9 ))	(list  gjbgs  (nth 6 gjb_list9 ))	(list  gjbzc (nth 7 gjb_list9 ) )	(list  gjbdwz (nth 8 gjb_list9 ) )	(list  gjbzl (nth 9 gjb_list9 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 10 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi10  (list (list  gjbbh (nth 0 gjb_list10 )) 	(list  gjbxs (nth 1 gjb_list10 ))	(list  gjbzj (nth 2 gjb_list10 ) )	(list  gjbjc (nth 3 gjb_list10 ) )	(list  gjbgc (nth 4 gjb_list10 ) ) 	(list  gjbdgc (nth 5 gjb_list10 ))	(list  gjbgs  (nth 6 gjb_list10 ))	(list  gjbzc (nth 7 gjb_list10 ) )	(list  gjbdwz (nth 8 gjb_list10 ) )	(list  gjbzl (nth 9 gjb_list10 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 11 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi11  (list (list  gjbbh (nth 0 gjb_list11 )) 	(list  gjbxs (nth 1 gjb_list11 ))	(list  gjbzj (nth 2 gjb_list11 ) )	(list  gjbjc (nth 3 gjb_list11 ) )	(list  gjbgc (nth 4 gjb_list11 ) ) 	(list  gjbdgc (nth 5 gjb_list11 ))	(list  gjbgs  (nth 6 gjb_list11 ))	(list  gjbzc (nth 7 gjb_list11 ) )	(list  gjbdwz (nth 8 gjb_list11 ) )	(list  gjbzl (nth 9 gjb_list11 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 12 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi12  (list (list  gjbbh (nth 0 gjb_list12 )) 	(list  gjbxs (nth 1 gjb_list12 ))	(list  gjbxs1 (nth 2 gjb_list12 )) (list  gjbzj (nth 3 gjb_list12 ) )	(list  gjbjc (nth 4 gjb_list12 ) )	(list  gjbgc (nth 5 gjb_list12 ) ) 	(list  gjbdgc (nth 6 gjb_list12 ))	(list  gjbgs  (nth 7 gjb_list12 ))	(list  gjbzc (nth 8 gjb_list12 ) )	(list  gjbdwz (nth 9 gjb_list12 ) )	(list  gjbzl (nth 10 gjb_list12 ) ))
	);line fuzi finish
	(setq  	  gjbbh 13 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi13  (list (list  gjbbh (nth 0 gjb_list13 )) 	(list  gjbxs (nth 1 gjb_list13 ))	 (list  gjbzj (nth 2 gjb_list13 ) )	(list  gjbjc (nth 3 gjb_list13 ) )	(list  gjbgc (nth 4 gjb_list13 ) ) 	(list  gjbdgc (nth 5 gjb_list13 ))	(list  gjbgs  (nth 6 gjb_list13))	(list  gjbzc (nth 7 gjb_list13 ) )	(list  gjbdwz (nth 8 gjb_list13 ) )	(list  gjbzl (nth 9 gjb_list13 ) ))
	);line fuzi finish
	(setq  	  gjbbh 14 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi14  (list (list  gjbbh (nth 0 gjb_list14 )) 	(list  gjbxs (nth 1 gjb_list14 ))	(list  gjbzj (nth 2 gjb_list14 ) )	(list  gjbjc (nth 3 gjb_list14 ) )	(list  gjbgc (nth 4 gjb_list14 ) ) 	(list  gjbdgc (nth 5 gjb_list14 ))	(list  gjbgs  (nth 6 gjb_list14 ))	(list  gjbzc (nth 7 gjb_list14 ) )	(list  gjbdwz (nth 8 gjb_list14 ) )	(list  gjbzl (nth 9 gjb_list14 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 15 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi15  (list (list  gjbbh (nth 0 gjb_list15 )) 	(list  gjbxs (nth 1 gjb_list15 ))	(list  gjbzj (nth 2 gjb_list15 ) )	(list  gjbjc (nth 3 gjb_list15 ) )	(list  gjbgc (nth 4 gjb_list15 ) ) 	(list  gjbdgc (nth 5 gjb_list15 ))	(list  gjbgs  (nth 6 gjb_list15 ))	(list  gjbzc (nth 7 gjb_list15 ) )	(list  gjbdwz (nth 8 gjb_list15 ) )	(list  gjbzl (nth 9 gjb_list15 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 16 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi16  (list (list  gjbbh (nth 0 gjb_list16 )) 	(list  gjbxs (nth 1 gjb_list16 ))	(list  gjbzj (nth 2 gjb_list16 ) )	(list  gjbjc (nth 3 gjb_list16 ) )	(list  gjbgc (nth 4 gjb_list16 ) ) 	(list  gjbdgc (nth 5 gjb_list16 ))	(list  gjbgs  (nth 6 gjb_list16 ))	(list  gjbzc (nth 7 gjb_list16 ) )	(list  gjbdwz (nth 8 gjb_list16 ) )	(list  gjbzl (nth 9 gjb_list16 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 17 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi17  (list (list  gjbbh (nth 0 gjb_list17 )) 	(list  gjbxs (nth 1 gjb_list17 ))	(list  gjbzj (nth 2 gjb_list17 ) )	(list  gjbjc (nth 3 gjb_list17 ) )	(list  gjbgc (nth 4 gjb_list17 ) ) 	(list  gjbdgc (nth 5 gjb_list17 ))	(list  gjbgs  (nth 6 gjb_list17 ))	(list  gjbzc (nth 7 gjb_list17 ) )	(list  gjbdwz (nth 8 gjb_list17 ) )	(list  gjbzl (nth 9 gjb_list17 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 18 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi18  (list (list  gjbbh (nth 0 gjb_list18 )) 	(list  gjbxs (nth 1 gjb_list18 ))	(list  gjbzj (nth 2 gjb_list18 ) )	(list  gjbjc (nth 3 gjb_list18 ) )	(list  gjbgc (nth 4 gjb_list18 ) ) 	(list  gjbdgc (nth 5 gjb_list18 ))	(list  gjbgs  (nth 6 gjb_list18 ))	(list  gjbzc (nth 7 gjb_list18 ) )	(list  gjbdwz (nth 8 gjb_list18 ) )	(list  gjbzl (nth 9 gjb_list18 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 19 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi19  (list (list  gjbbh (nth 0 gjb_list19 )) 	(list  gjbxs (nth 1 gjb_list19 ))	(list  gjbzj (nth 2 gjb_list19 ) )	(list  gjbjc (nth 3 gjb_list19 ) )	(list  gjbgc (nth 4 gjb_list19 ) ) 	(list  gjbdgc (nth 5 gjb_list19 ))	(list  gjbgs  (nth 6 gjb_list19 ))	(list  gjbzc (nth 7 gjb_list19 ) )	(list  gjbdwz (nth 8 gjb_list19 ) )	(list  gjbzl (nth 9 gjb_list19 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 20 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi20  (list (list  gjbbh (nth 0 gjb_list20 )) 	(list  gjbxs (nth 1 gjb_list20 ))	(list  gjbzj (nth 2 gjb_list20 ) )	(list  gjbjc (nth 3 gjb_list20 ) )	(list  gjbgc (nth 4 gjb_list20 ) ) 	(list  gjbdgc (nth 5 gjb_list20 ))	(list  gjbgs  (nth 6 gjb_list20 ))	(list  gjbzc (nth 7 gjb_list20 ) )	(list  gjbdwz (nth 8 gjb_list20 ) )	(list  gjbzl (nth 9 gjb_list20 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 21 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi21  (list (list  gjbbh (nth 0 gjb_list21 )) 	(list  gjbxs (nth 1 gjb_list21 ))	(list  gjbzj (nth 2 gjb_list21 ) )	(list  gjbjc (nth 3 gjb_list21 ) )	(list  gjbgc (nth 4 gjb_list21 ) ) 	(list  gjbdgc (nth 5 gjb_list21 ))	(list  gjbgs  (nth 6 gjb_list21 ))	(list  gjbzc (nth 7 gjb_list21 ) )	(list  gjbdwz (nth 8 gjb_list21 ) )	(list  gjbzl (nth 9 gjb_list21 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 22 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi22  (list (list  gjbbh (nth 0 gjb_list22 )) 	(list  gjbxs (nth 1 gjb_list22 ))	(list  gjbzj (nth 2 gjb_list22 ) )	(list  gjbjc (nth 3 gjb_list22 ) )	(list  gjbgc (nth 4 gjb_list22 ) ) 	(list  gjbdgc (nth 5 gjb_list22 ))	(list  gjbgs  (nth 6 gjb_list22 ))	(list  gjbzc (nth 7 gjb_list22 ) )	(list  gjbdwz (nth 8 gjb_list22 ) )	(list  gjbzl (nth 9 gjb_list22 ) ))
	);line fuzi finish
	(setq  	  gjbbh 23 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi23  (list (list  gjbbh (nth 0 gjb_list23 )) 	(list  gjbxs (nth 1 gjb_list23 ))	(list  gjbzj (nth 2 gjb_list23 ) )	(list  gjbjc (nth 3 gjb_list23 ) )	(list  gjbgc (nth 4 gjb_list23 ) ) 	(list  gjbdgc (nth 5 gjb_list23 ))	(list  gjbgs  (nth 6 gjb_list23 ))	(list  gjbzc (nth 7 gjb_list23 ) )	(list  gjbdwz (nth 8 gjb_list23 ) )	(list  gjbzl (nth 9 gjb_list23 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 24 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi24  (list (list  gjbbh (nth 0 gjb_list24 )) 	(list  gjbxs (nth 1 gjb_list24 ))	(list  gjbzj (nth 2 gjb_list24 ) )	(list  gjbjc (nth 3 gjb_list24 ) )	(list  gjbgc (nth 4 gjb_list24 ) ) 	(list  gjbdgc (nth 5 gjb_list24 ))	(list  gjbgs  (nth 6 gjb_list24 ))	(list  gjbzc (nth 7 gjb_list24 ) )	(list  gjbdwz (nth 8 gjb_list24 ) )	(list  gjbzl (nth 9 gjb_list24 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 25 	  gjbxs (- l1 50) gjbxs1 450	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi25  (list (list  gjbbh (nth 0 gjb_list25 )) 	(list  gjbxs (nth 1 gjb_list25 ))	(list  gjbxs1 (nth 2 gjb_list25 )) (list  gjbzj (nth 3 gjb_list25 ) )	(list  gjbjc (nth 4 gjb_list25 ) )	(list  gjbgc (nth 5 gjb_list25 ) ) 	(list  gjbdgc (nth 6 gjb_list25 ))	(list  gjbgs  (nth 7 gjb_list25 ))	(list  gjbzc (nth 8 gjb_list25 ) )	(list  gjbdwz (nth 9 gjb_list25 ) )	(list  gjbzl (nth 10 gjb_list25 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 26 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi26  (list (list  gjbbh (nth 0 gjb_list26 )) 	(list  gjbxs (nth 1 gjb_list26 ))	(list  gjbzj (nth 2 gjb_list26 ) )	(list  gjbjc (nth 3 gjb_list26 ) )	(list  gjbgc (nth 4 gjb_list26 ) ) 	(list  gjbdgc (nth 5 gjb_list26 ))	(list  gjbgs  (nth 6 gjb_list26 ))	(list  gjbzc (nth 7 gjb_list26 ) )	(list  gjbdwz (nth 8 gjb_list26 ) )	(list  gjbzl (nth 9 gjb_list26 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 27 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi27  (list (list  gjbbh (nth 0 gjb_list27 )) 	(list  gjbxs (nth 1 gjb_list27 ))	(list  gjbzj (nth 2 gjb_list27 ) )	(list  gjbjc (nth 3 gjb_list27 ) )	(list  gjbgc (nth 4 gjb_list27 ) ) 	(list  gjbdgc (nth 5 gjb_list27 ))	(list  gjbgs  (nth 6 gjb_list27 ))	(list  gjbzc (nth 7 gjb_list27 ) )	(list  gjbdwz (nth 8 gjb_list27 ) )	(list  gjbzl (nth 9 gjb_list27 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 28 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi28  (list (list  gjbbh (nth 0 gjb_list28 )) 	(list  gjbxs (nth 1 gjb_list28 ))	(list  gjbzj (nth 2 gjb_list28 ) )	(list  gjbjc (nth 3 gjb_list28 ) )	(list  gjbgc (nth 4 gjb_list28 ) ) 	(list  gjbdgc (nth 5 gjb_list28 ))	(list  gjbgs  (nth 6 gjb_list28 ))	(list  gjbzc (nth 7 gjb_list28 ) )	(list  gjbdwz (nth 8 gjb_list28 ) )	(list  gjbzl (nth 9 gjb_list28 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 29 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi29  (list (list  gjbbh (nth 0 gjb_list29 )) 	(list  gjbxs (nth 1 gjb_list29 ))	(list  gjbzj (nth 2 gjb_list29 ) )	(list  gjbjc (nth 3 gjb_list29 ) )	(list  gjbgc (nth 4 gjb_list29 ) ) 	(list  gjbdgc (nth 5 gjb_list29 ))	(list  gjbgs  (nth 6 gjb_list29 ))	(list  gjbzc (nth 7 gjb_list29 ) )	(list  gjbdwz (nth 8 gjb_list29 ) )	(list  gjbzl (nth 9 gjb_list29 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 30 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi30  (list (list  gjbbh (nth 0 gjb_list30 )) 	(list  gjbxs (nth 1 gjb_list30 ))	(list  gjbzj (nth 2 gjb_list30 ) )	(list  gjbjc (nth 3 gjb_list30 ) )	(list  gjbgc (nth 4 gjb_list30 ) ) 	(list  gjbdgc (nth 5 gjb_list30 ))	(list  gjbgs  (nth 6 gjb_list30 ))	(list  gjbzc (nth 7 gjb_list30 ) )	(list  gjbdwz (nth 8 gjb_list30 ) )	(list  gjbzl (nth 9 gjb_list30 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 31 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi31  (list (list  gjbbh (nth 0 gjb_list31 )) 	(list  gjbxs (nth 1 gjb_list31 ))	(list  gjbzj (nth 2 gjb_list31 ) )	(list  gjbjc (nth 3 gjb_list31 ) )	(list  gjbgc (nth 4 gjb_list31) ) 	(list  gjbdgc (nth 5 gjb_list31 ))	(list  gjbgs  (nth 6 gjb_list31 ))	(list  gjbzc (nth 7 gjb_list31 ) )	(list  gjbdwz (nth 8 gjb_list31 ) )	(list  gjbzl (nth 9 gjb_list31 ) ))
	);line fuzi finish
	(setq  	  gjbbh 32 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi32  (list (list  gjbbh (nth 0 gjb_list32 )) 	(list  gjbxs (nth 1 gjb_list32 ))	(list  gjbzj (nth 2 gjb_list32 ) )	(list  gjbjc (nth 3 gjb_list32 ) )	(list  gjbgc (nth 4 gjb_list32) ) 	(list  gjbdgc (nth 5 gjb_list32 ))	(list  gjbgs  (nth 6 gjb_list32 ))	(list  gjbzc (nth 7 gjb_list32 ) )	(list  gjbdwz (nth 8 gjb_list32 ) )	(list  gjbzl (nth 9 gjb_list32 ) )))
	;32 finish
	(setq  	  gjbbh 33 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi33  (list (list  gjbbh (nth 0 gjb_list33 )) 	(list  gjbxs (nth 1 gjb_list33 ))	(list  gjbzj (nth 2 gjb_list33 ) )	(list  gjbjc (nth 3 gjb_list33 ) )	(list  gjbgc (nth 4 gjb_list33 ) ) 	(list  gjbdgc (nth 5 gjb_list33 ))	(list  gjbgs  (nth 6 gjb_list33 ))	(list  gjbzc (nth 7 gjb_list33 ) )	(list  gjbdwz (nth 8 gjb_list33 ) )	(list  gjbzl (nth 9 gjb_list33 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 34 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi34  (list (list  gjbbh (nth 0 gjb_list34 )) 	(list  gjbxs (nth 1 gjb_list34 ))	(list  gjbzj (nth 2 gjb_list34 ) )	(list  gjbjc (nth 3 gjb_list34 ) )	(list  gjbgc (nth 4 gjb_list34 ) ) 	(list  gjbdgc (nth 5 gjb_list34 ))	(list  gjbgs  (nth 6 gjb_list34 ))	(list  gjbzc (nth 7 gjb_list34 ) )	(list  gjbdwz (nth 8 gjb_list34 ) )	(list  gjbzl (nth 9 gjb_list34 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 35 	  gjbxs (- l1 50) 	gjbxs1 500  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi35  (list (list  gjbbh (nth 0 gjb_list35 )) 	(list  gjbxs (nth 1 gjb_list35 ))	(list  gjbxs1 (nth 2 gjb_list35 )) (list  gjbzj (nth 3 gjb_list35 ) )	(list  gjbjc (nth 4 gjb_list35 ) )	(list  gjbgc (nth 5 gjb_list35 ) ) 	(list  gjbdgc (nth 6 gjb_list35 ))	(list  gjbgs  (nth 7 gjb_list35 ))	(list  gjbzc (nth 8 gjb_list35 ) )	(list  gjbdwz (nth 9 gjb_list35 ) )	(list  gjbzl (nth 10 gjb_list35 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 36 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi36  (list (list  gjbbh (nth 0 gjb_list36 )) 	(list  gjbxs (nth 1 gjb_list36 ))	(list  gjbzj (nth 2 gjb_list36 ) )	(list  gjbjc (nth 3 gjb_list36 ) )	(list  gjbgc (nth 4 gjb_list36 ) ) 	(list  gjbdgc (nth 5 gjb_list36 ))	(list  gjbgs  (nth 6 gjb_list36 ))	(list  gjbzc (nth 7 gjb_list36 ) )	(list  gjbdwz (nth 8 gjb_list36 ) )	(list  gjbzl (nth 9 gjb_list36 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 37 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi37  (list (list  gjbbh (nth 0 gjb_list37 )) 	(list  gjbxs (nth 1 gjb_list37 ))	(list  gjbzj (nth 2 gjb_list37 ) )	(list  gjbjc (nth 3 gjb_list37 ) )	(list  gjbgc (nth 4 gjb_list37 ) ) 	(list  gjbdgc (nth 5 gjb_list37 ))	(list  gjbgs  (nth 6 gjb_list37 ))	(list  gjbzc (nth 7 gjb_list37 ) )	(list  gjbdwz (nth 8 gjb_list37 ) )	(list  gjbzl (nth 9 gjb_list37 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 38 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi38  (list (list  gjbbh (nth 0 gjb_list38 )) 	(list  gjbxs (nth 1 gjb_list38 ))	(list  gjbzj (nth 2 gjb_list38 ) )	(list  gjbjc (nth 3 gjb_list38 ) )	(list  gjbgc (nth 4 gjb_list38 ) ) 	(list  gjbdgc (nth 5 gjb_list38 ))	(list  gjbgs  (nth 6 gjb_list38 ))	(list  gjbzc (nth 7 gjb_list38 ) )	(list  gjbdwz (nth 8 gjb_list38 ) )	(list  gjbzl (nth 9 gjb_list38 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 39 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi39  (list (list  gjbbh (nth 0 gjb_list39 )) 	(list  gjbxs (nth 1 gjb_list39 ))	(list  gjbzj (nth 2 gjb_list39 ) )	(list  gjbjc (nth 3 gjb_list39 ) )	(list  gjbgc (nth 4 gjb_list39 ) ) 	(list  gjbdgc (nth 5 gjb_list39 ))	(list  gjbgs  (nth 6 gjb_list39 ))	(list  gjbzc (nth 7 gjb_list39 ) )	(list  gjbdwz (nth 8 gjb_list39 ) )	(list  gjbzl (nth 9 gjb_list39 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 40 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi40  (list (list  gjbbh (nth 0 gjb_list40 )) 	(list  gjbxs (nth 1 gjb_list40 ))	(list  gjbzj (nth 2 gjb_list40 ) )	(list  gjbjc (nth 3 gjb_list40 ) )	(list  gjbgc (nth 4 gjb_list40 ) ) 	(list  gjbdgc (nth 5 gjb_list40 ))	(list  gjbgs  (nth 6 gjb_list40 ))	(list  gjbzc (nth 7 gjb_list40 ) )	(list  gjbdwz (nth 8 gjb_list40 ) )	(list  gjbzl (nth 9 gjb_list40 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 41 	  gjbxs (- l1 50) gjbxs1 500	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi41  (list (list  gjbbh (nth 0 gjb_list41 )) 	(list  gjbxs (nth 1 gjb_list41 ))	(list  gjbxs1 (nth 2 gjb_list41 )) (list  gjbzj (nth 3 gjb_list41 ) )	(list  gjbjc (nth 4 gjb_list41 ) )	(list  gjbgc (nth 5 gjb_list41 ) ) 	(list  gjbdgc (nth 6 gjb_list41 ))	(list  gjbgs  (nth 7 gjb_list41 ))	(list  gjbzc (nth 8 gjb_list41 ) )	(list  gjbdwz (nth 9 gjb_list41 ) )	(list  gjbzl (nth 10 gjb_list41 ) ))
	);line fuzi finish
	(setq  	  gjbbh 42	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi42  (list (list  gjbbh (nth 0 gjb_list42 )) 	(list  gjbxs (nth 1 gjb_list42 ))	(list  gjbzj (nth 2 gjb_list42 ) )	(list  gjbjc (nth 3 gjb_list42 ) )	(list  gjbgc (nth 4 gjb_list42 ) ) 	(list  gjbdgc (nth 5 gjb_list42 ))	(list  gjbgs  (nth 6 gjb_list42 ))	(list  gjbzc (nth 7 gjb_list42 ) )	(list  gjbdwz (nth 8 gjb_list42 ) )	(list  gjbzl (nth 9 gjb_list42 ) ))
	);line fuzi finish
	
	
	
	(setq  	  gjbbh 43 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi43  (list (list  gjbbh (nth 0 gjb_list43 )) 	(list  gjbxs (nth 1 gjb_list43 ))	(list  gjbzj (nth 2 gjb_list43 ) )	(list  gjbjc (nth 3 gjb_list43 ) )	(list  gjbgc (nth 4 gjb_list43 ) ) 	(list  gjbdgc (nth 5 gjb_list43 ))	(list  gjbgs  (nth 6 gjb_list43 ))	(list  gjbzc (nth 7 gjb_list43 ) )	(list  gjbdwz (nth 8 gjb_list43 ) )	(list  gjbzl (nth 9 gjb_list43 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 44 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi44  (list (list  gjbbh (nth 0 gjb_list44 )) 	(list  gjbxs (nth 1 gjb_list44 ))	(list  gjbzj (nth 2 gjb_list44 ) )	(list  gjbjc (nth 3 gjb_list44 ) )	(list  gjbgc (nth 4 gjb_list44 ) ) 	(list  gjbdgc (nth 5 gjb_list44 ))	(list  gjbgs  (nth 6 gjb_list44 ))	(list  gjbzc (nth 7 gjb_list44 ) )	(list  gjbdwz (nth 8 gjb_list44 ) )	(list  gjbzl (nth 9 gjb_list44 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 45 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi45  (list (list  gjbbh (nth 0 gjb_list45 )) 	(list  gjbxs (nth 1 gjb_list45 ))	(list  gjbzj (nth 2 gjb_list45 ) )	(list  gjbjc (nth 3 gjb_list45 ) )	(list  gjbgc (nth 4 gjb_list45 ) ) 	(list  gjbdgc (nth 5 gjb_list45 ))	(list  gjbgs  (nth 6 gjb_list45 ))	(list  gjbzc (nth 7 gjb_list45 ) )	(list  gjbdwz (nth 8 gjb_list45 ) )	(list  gjbzl (nth 9 gjb_list45 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 46 	  gjbxs (- l1 50) 	gjbxs1 450  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi46  (list (list  gjbbh (nth 0 gjb_list46 )) 	(list  gjbxs (nth 1 gjb_list46 ))	(list  gjbxs1 (nth 2 gjb_list46 )) (list  gjbzj (nth 3 gjb_list46 ) )	(list  gjbjc (nth 4 gjb_list46 ) )	(list  gjbgc (nth 5 gjb_list46 ) ) 	(list  gjbdgc (nth 6 gjb_list46 ))	(list  gjbgs  (nth 7 gjb_list46 ))	(list  gjbzc (nth 8 gjb_list46 ) )	(list  gjbdwz (nth 9 gjb_list46 ) )	(list  gjbzl (nth 10 gjb_list46 ) ))
	);line fuzi finish
	
	(setq  	  gjbbh 47 	  gjbxs (- l1 50) 	  gjbzj 12 	  gjbjc gjbxs 	  gjbgc 0 	  gjbdgc (+ gjbjc gjbgc) 	  gjbgs  (fix (1+  (/ h2 200))) 	  gjbzc (rtos (* gjbgs gjbdgc) 2 0) 	  gjbdwz (rtos 0.888 2 3) 	  gjbzl (rtos (/ (* (atof gjbdwz) (atof gjbzc)) 1000) 2 2));rtos 实数取２位
	
	(setq gjbfuzi47  (list (list  gjbbh (nth 0 gjb_list47 )) 	(list  gjbxs (nth 1 gjb_list47 ))	(list  gjbzj (nth 2 gjb_list47 ) )	(list  gjbjc (nth 3 gjb_list47 ) )	(list  gjbgc (nth 4 gjb_list47 ) ) 	(list  gjbdgc (nth 5 gjb_list47 ))	(list  gjbgs  (nth 6 gjb_list47 ))	(list  gjbzc (nth 7 gjb_list47 ) )	(list  gjbdwz (nth 8 gjb_list47 ) )	(list  gjbzl (nth 9 gjb_list47 ) ))
	);line fuzi finish
	
	
  
	(setq gjbzb0 (list gjbfuzi1  gjbfuzi2  gjbfuzi3  gjbfuzi4  gjbfuzi5  gjbfuzi6  gjbfuzi7  gjbfuzi8  gjbfuzi9  gjbfuzi10  gjbfuzi11  gjbfuzi12  gjbfuzi13  gjbfuzi14  gjbfuzi15  gjbfuzi16  gjbfuzi17  gjbfuzi18  gjbfuzi19  gjbfuzi20  gjbfuzi21  gjbfuzi22  gjbfuzi23  gjbfuzi24  gjbfuzi25  gjbfuzi26  gjbfuzi27  gjbfuzi28  gjbfuzi29  gjbfuzi30  gjbfuzi31  gjbfuzi32  gjbfuzi33  gjbfuzi34  gjbfuzi35  gjbfuzi36  gjbfuzi37  gjbfuzi38  gjbfuzi39  gjbfuzi40  gjbfuzi41  gjbfuzi42  gjbfuzi43  gjbfuzi44  gjbfuzi45  gjbfuzi46  gjbfuzi47   ))
	
	(draw-gjb)
);defun gjbfuzi finish

(defun draw-gjb();draw gjb progman biaoz 函数给biaozhi000 形成了一个表，储存了每个文字的坐标，取得文字的值 按坐标写入。
	
  (setvar "cmdecho" 0)
  (setq jidian (list 47000 111400))
  (setq txt	"D:\\yhdlisp\\bz\\biaoyangnoline.dwg")
	;(getfiled "坐标点" "D:\\yhdlisp\\bz\\" "dwg" 2);只能打开在选项中支持的搜索目录，如上面的;
  (command "insert" txt jidian "" "" 0 );
	;(command "insert" txt jidian  100 ""  0 "")不能放大，表格文字坐标还行调整，以后缩放得了	
  (setvar "cmdecho" 1)
  (setq i 0)
  (setq gjbjs001 (list (nth 0 jidian) (nth 1 jidian)))
	
  (repeat 47 ;写数值，先取出行数据 ，然后一行一行写
		(setq bi (nth i gjbzb0))
		(setq i (1+ i))
		(setq n (length bi))		; 获得每行的文字数。
		(setq nx 0)
		(repeat n
			(setq gjbzb00 (mapcar '+ gjbjs001 (nth 1 (nth nx bi))	)	);求文字坐标
			(setq gjbz000 (car (nth nx bi)));word content
			;(if (>= nx 7) (setq gjbz000 (rtos gjbz000)) );如果是总长（实数型数）等换成字符串，取消末尾的零
			;(if (>= nx 7) (setq gjbz000 (+ gjbz000 0.000)))
			(command 		"_text"		"j" "bl"		gjbzb00	 3 0	gjbz000	);(itoa gjbz000)
			(setq nx (1+ nx));控制 0-10列的写
			
		);repeat n 结束
	);repeat 47 finish.(command
	(command "zoom" "a")
	(command "scale" "c" jidian "@297,-420" "" jidian  100 "")
	
);defun end

(defun xgjjs	(xgjx xgjs pint / lls )		;线钢筋坐标点计算 xgjx xgjs  分别为下点和上点坐标，必记得 pint 为标示计算方向，见图
	(setq lls (abs (distance xgjx xgjs)))
  (setq gshu (+ (fix (+ (/ lls jjgj) 0.5)) 1)) ;钢筋根数gshu
	
  (if
		(= pint 0)
		(progn
			(setq ptx ls)
			(setq pty hbaohu)
			(setq pt1 (zbjs xgjx ptx pty))	;(第一条钢筋线的基点坐标
			(setq pty (* pty -1))
			(setq pt2 (zbjs xgjs ptx pty))
			(setq ptx ls1)
			(setq pty (+ hbaohu (/ rgj 2)))
			(setq pt01 (zbjs xgjx ptx pty))
			(setq pty (* pty -1))
			(setq pt02 (zbjs xgjs ptx pty))
		)
  )
  (if
		(= pint 1) ;绘竖钢筋，点钢筋在左。
		(progn
			(setq ptx (* -1 ls))
			(setq pty hbaohu)
			(setq pt1 (zbjs xgjx ptx pty))	;(第一条钢筋线的基点坐标
			(setq pty (* pty -1))
			(setq pt2 (zbjs xgjs ptx pty))
			(setq ptx (* -1 ls1))
			(setq pty (+ hbaohu (/ rgj 2)))
			(setq pt01 (zbjs xgjx ptx pty))
			(setq pty (* pty -1))
			(setq pt02 (zbjs xgjs ptx pty))
		)
  )
  (if
		(= pint 2) ;绘横钢筋，点钢筋在上。
		(progn
			(setq ptx hbaohu)
			(setq pty ls)
			(setq pt1 (zbjs xgjx ptx pty))	;(第一条钢筋线的基点坐标
			(setq ptx (* -1 ptx))
			(setq pt2 (zbjs xgjs ptx pty))
			(setq ptx (+ hbaohu (/ rgj 2)))
			(setq pty ls1)
			(setq pt01 (zbjs xgjx ptx pty))
			(setq ptx (* -1 ptx))
			(setq pt02 (zbjs xgjs ptx pty))
		)
  )
  (if
		(= pint 3) ;绘横钢筋，点钢筋在下。
		(progn
			(setq ptx hbaohu)
			(setq pty (* -1 ls))
			(setq pt1 (zbjs xgjx ptx pty))	;(第一条钢筋线的基点坐标
			(setq ptx (* -1 ptx))
			(setq pt2 (zbjs xgjs ptx pty))
			(setq ptx (+ hbaohu (/ rgj 2)))
			(setq pty (* -1 ls1))
			(setq pt01 (zbjs xgjx ptx pty))
			(setq ptx (* -1 ptx))
			(setq pt02 (zbjs xgjs ptx pty))
		)
		
  )
	(if
		(= pint 4) ;绘横钢筋，点钢筋在下。横钢筋在上，但是点钢筋在边线和线钢筋中间。
		(progn
			(setq ptx hbaohu)
			(setq pty (+ ls rgj));保护层+线宽一半+点钢筋直径
			(setq pt1 (zbjs xgjx ptx pty))	;(第一条钢筋线的基点坐标
			(setq ptx (* -1 ptx))
			(setq pt2 (zbjs xgjs ptx pty))
			(setq ptx (+ hbaohu (/ rgj 2)))
			(setq pty (+ hbaohu (/ rgj 2)))
			(setq pt01 (zbjs xgjx ptx pty))
			(setq ptx (* -1 ptx))
			(setq pt02 (zbjs xgjs ptx pty))
		))
	(setq jjgs1 (/ (abs (distance pt01 pt02)) (- gshu 1)))	;应该实际调整间距
)


(defun gshujs	(pt1 pt2 )	
	
	;计算钢筋根数,lgj钢筋长，jjgj钢筋间距，rgj 点钢筋半径。
	;********
  (setq lls (abs (distance pt1 pt2)))
  (setq gshu (+ (fix (+ (/ lls jjgj) 0.5)) 1)) ;钢筋根数gshu
  (setq jjgs1 (/ lls (- gshu 1)))	;应该实际调整间距。
)

(defun gsjs 	(lgs bhgs jjgs ptgs)	;
	;********
	;阵列画点。gshu 是钢筋根数，LGS长指线长，不是钢筋长，JJgs钢筋间距，JJgs1点钢筋间距。这个是为了取整，四舍五入后。得间距
	;********
  (setq
		gshu (+ (fix (+ (/ (- lgs bhgs bhgs) jjgs) 0.5)) 1)
  )
  (if
		(> gshu 1)
		(progn
			(command "donut" 0 rgj ptgs "")
			(setq jjgs1 (/ (- lgs bhgs bhgs) (- gshu 1)))
			(command "array" "l" "" "r" gshu 1 jjgs1)
			；
		) 	;progn end
  ) 	;if end
)
;********

(defun gsjsh	(lgs bhgs jjgs ptgs)	
	;gshu 是钢筋根数，LGS长指线长，不是钢筋长，JJgs钢筋间距，JJgs1点钢筋间距。这个是为了取整，四舍五入后。得间距
	;********
  (setq
		gshu (+ (fix (+ (/ (- lgs bhgs bhgs) jjgs) 0.5)) 1)
  )
  (if
		(> gshu 1)
		(progn
			(command "donut" 0 rgj ptgs "")
			(setq jjgs1 (/ (- lgs bhgs bhgs) (- gshu 1)))
			(command "array" "l" "" "r" 1 gshu jjgs1)
			；
		) 	;progn end
  ) 	;if end
)
(defun bz1
	(pt1 pt2 / dis1)
  (setq
		dis1 (- (/ (distance pt1 pt2) 2) hbaohu hdpl (/ rgj 2))
  )
)

(defun huagjd
	(B)
  (repeat
		gshu
		(command "donut" 0 rgj jid00 "")
		(command "donut" 0 rgj jid000 "")
		(setq pdb (polar (zd jid00 jid000) B 120))
		;直接输入在中线上的长120，小于200间距，
		(command "line" jid00 pdb jid000 "")
		(setq jid00 (zbjs jid00 ptx pty))
		(setq jid000 (zbjs jid000 ptx pty))
  )
)
(defun huagjd1
	(B)			;只画一条线，
  (repeat
		gshu
		(command "donut" 0 rgj jid00 "")
		(command "line" jid00 pdb "")
		(setq pdb (zbjs pdb 0 200))	;间距直接就是200，以后修正为jjgj,先进行，不然没信心做下去了
		(setq jid00 (zbjs jid00 0 200))
  )
)

;(defun write_bt(/ str-zf str-l)
;	(setq str-l (strlen str-bt ))
;	(command "cplay" "2")
;	(command "lw" 0.4)
;	(command "pline" str_point (polar str_point 0 str-l)  "")
;	(command "lw" "blayer");blayer
;	(command "pline" (polar  str_point (* pi -0.5) 150) str-l "" )
;	(command "text" "j" "mL" str-point 50 0 str-zf );(command "text" "j" "mL" linpd0 50 0 (strcat (itoa bianhao) "  %%130 " (itoa zhijing) " @200"))
;)

(defun biaoz	() ;对钢筋表赋值函数，每一行是一个表，是坐标，
  
	
	
	(setq gjb_list1 (list  (list 94.0061 -18.7000) (list 114.7959 -18.7000) (list 139.0297 -18.7000) (list 158.6149 -18.7000) (list 175.5000 -18.7000) (list 195.3091 -18.7000) (list 212.6432 -18.7000) (list 225.4749 -18.7000) (list 245.0601 -18.7000) (list 265.0956 -18.7000) ))
	
	(setq gjb_list2 (list  (list 94.0061 -29.2600) (list 116.1180 -27.6616) (list 125.7617 -29.2600) (list 139.0297 -29.2600) (list 158.6149 -29.2600) (list 175.5000 -29.2600) (list 195.3091 -29.2600) (list 212.6432 -29.2600) (list 224.3493 -29.2600) (list 245.0601 -29.2600) (list 265.0956 -29.2600) ))
	
	(setq gjb_list3 (list  (list 94.0061 -38.3000) (list 114.7959 -38.3000) (list 140.3804 -38.3000) (list 158.6149 -38.3000) (list 175.5000 -38.3000) (list 194.1835 -38.3000) (list 212.6432 -38.3000) (list 225.4749 -38.3000) (list 245.0601 -38.3000) (list 266.2212 -38.3000) ))
	
	(setq gjb_list4 (list  (list 94.0061 -50.6900) (list 104.5918 -48.5000) (list 113.7379 -50.6900) (list 139.0297 -50.6900) (list 157.4893 -50.6900) (list 175.5000 -50.6900) (list 194.1835 -50.6900) (list 212.6432 -50.6900) (list 224.3493 -50.6900) (list 245.0601 -50.6900) (list 263.9700 -50.6900) ))
	
	(setq gjb_list5 (list  (list 94.0061 -58.3000) (list 114.7959 -58.3000) (list 140.3804 -58.3000) (list 157.4893 -58.3000) (list 175.5000 -58.3000) (list 194.1835 -58.3000) (list 212.6432 -58.3000) (list 224.3493 -58.3000) (list 245.0601 -58.3000) (list 265.0956 -58.3000) ))
	
	(setq gjb_list6 (list  (list 94.0061 -70.3000) (list 121.4806 -64.1709) (list 157.4893 -64.4000) (list 175.5000 -64.4000) (list 194.3200 -64.4000) (list 114.7959 -70.3000) (list 139.0297 -70.3000) (list 157.4893 -70.3000) (list 175.5000 -70.3000) (list 194.3200 -70.3000) (list 212.6432 -70.3000) (list 225.4749 -70.3000) (list 245.0601 -70.3000) (list 265.0956 -70.3000) ))
	
	(setq gjb_list7 (list  (list 94.0061 -83.2000) (list 125.7617 -83.2000) (list 139.0297 -83.2000) (list 157.4893 -83.2000) (list 175.5000 -83.2000) (list 194.5529 -83.2000) (list 212.6432 -83.2000) (list 224.3493 -83.2000) (list 245.0601 -83.2000) (list 265.0956 -83.2000) (list 116.1180 -77.0220) (list 175.5000 -77.7672) (list 157.4893 -77.7672) (list 194.5529 -77.7672) (list 125.7617 -78.2266) (list 116.1180 -82.0403) ))
	
	(setq gjb_list8 (list  (list 94.0061 -91.0000) (list 114.7959 -91.0000) (list 140.3804 -91.0000) (list 157.4893 -91.0000) (list 175.5000 -91.0000) (list 194.1835 -91.0000) (list 212.6432 -91.0000) (list 224.3493 -91.0000) (list 245.0601 -91.0000) (list 265.0956 -91.0000) ))
	
	(setq gjb_list9 (list  (list 94.0061 -100.0000) (list 114.7959 -100.0000) (list 140.3804 -100.0000) (list 157.4893 -100.0000) (list 175.5000 -100.0000) (list 194.1835 -100.0000) (list 212.6432 -100.0000) (list 225.4749 -100.0000) (list 245.0601 -100.0000) (list 266.2212 -100.0000) ))
	
	(setq gjb_list10 (list  (list 92.8805 -110.0000) (list 114.7959 -110.0000) (list 139.0297 -110.0000) (list 157.4893 -110.0000) (list 175.5000 -110.0000) (list 194.1835 -110.0000) (list 212.6432 -110.0000) (list 224.3493 -110.0000) (list 245.0601 -110.0000) (list 263.9700 -110.0000) ))
	
	(setq gjb_list11 (list  (list 92.8805 -117.0000) (list 114.7959 -117.0000) (list 140.3804 -117.0000) (list 157.4893 -117.0000) (list 175.5000 -117.0000) (list 194.1835 -117.0000) (list 212.6432 -117.0000) (list 223.4488 -117.0000) (list 245.0601 -117.0000) (list 265.0956 -117.0000) ))
	
	(setq gjb_list12 (list  (list 92.8805 -129.2000) (list 105.0020 -127.0000) (list 115.0654 -129.2000) (list 139.0297 -129.2000) (list 157.4893 -129.2000) (list 175.5000 -129.2000) (list 194.1835 -129.2000) (list 212.6432 -129.2000) (list 227.7261 -129.2000) (list 245.0601 -129.2000) (list 265.0956 -129.2000) ))
	
	(setq gjb_list13 (list  (list 92.8805 -136.5000) (list 114.7959 -136.5000) (list 140.3804 -136.5000) (list 157.4893 -136.5000) (list 175.5000 -136.5000) (list 194.1835 -136.5000) (list 213.7688 -136.5000) (list 225.4749 -136.5000) (list 245.0601 -136.5000) (list 266.2212 -136.5000) ))
	
	(setq gjb_list14 (list  (list 92.8805 -142.5000) (list 114.7959 -142.5000) (list 139.0297 -142.5000) (list 157.4893 -142.5000) (list 175.5000 -142.5000) (list 194.1835 -142.5000) (list 212.6432 -142.5000) (list 227.7261 -142.5000) (list 245.0601 -142.5000) (list 265.0956 -142.5000) ))
	
	(setq gjb_list15 (list  (list 92.8805 -150.0000) (list 114.7959 -150.0000) (list 139.0297 -150.0000) (list 157.4893 -150.0000) (list 175.5000 -150.0000) (list 194.1835 -150.0000) (list 212.6432 -150.0000) (list 225.4749 -150.0000) (list 245.0601 -150.0000) (list 265.0956 -150.0000) ))
	
	(setq gjb_list16 (list  (list 92.8805 -156.3000) (list 114.7959 -156.3000) (list 139.0297 -156.3000) (list 157.4893 -156.3000) (list 175.5000 -156.3000) (list 194.1835 -156.3000) (list 212.6432 -156.3000) (list 227.7261 -156.3000) (list 249.5625 -156.3000) (list 263.9700 -156.3000) ))
	
	(setq gjb_list17 (list  (list 92.8805 -162.5000) (list 114.7959 -162.5000) (list 139.0297 -162.5000) (list 157.4893 -162.5000) (list 175.5000 -162.5000) (list 194.1835 -162.5000) (list 212.6432 -162.5000) (list 225.4749 -162.5000) (list 249.5625 -162.5000) (list 263.9700 -162.5000) ))
	
	(setq gjb_list18 (list  (list 92.8805 -169.3000) (list 114.7959 -169.3000) (list 139.0297 -169.3000) (list 157.4893 -169.3000) (list 175.5000 -169.3000) (list 194.1835 -169.3000) (list 212.6432 -169.3000) (list 224.3493 -169.3000) (list 245.0601 -169.3000) (list 263.9700 -169.3000) ))
	
	(setq gjb_list19 (list  (list 92.8805 -176.1000) (list 114.7959 -176.1000) (list 140.3804 -176.1000) (list 157.4893 -176.1000) (list 175.5000 -176.1000) (list 194.1835 -176.1000) (list 212.6432 -176.1000) (list 225.4749 -176.1000) (list 245.0601 -176.1000) (list 265.0956 -176.1000) ))
	
	(setq gjb_list20 (list  (list 92.8805 -182.1000) (list 114.7959 -182.1000) (list 139.0297 -182.1000) (list 157.4893 -182.1000) (list 175.5000 -182.1000) (list 194.1835 -182.1000) (list 212.6432 -182.1000) (list 226.6005 -182.1000) (list 245.0601 -182.1000) (list 263.9700 -182.1000) ))
	
	(setq gjb_list21 (list  (list 92.8805 -188.6000) (list 114.7959 -188.6000) (list 140.3804 -188.6000) (list 157.4893 -188.6000) (list 175.5000 -188.6000) (list 194.1835 -188.6000) (list 212.6432 -188.6000) (list 226.6005 -188.6000) (list 245.0601 -188.6000) (list 265.0956 -188.6000) ))
	
	(setq gjb_list22 (list  (list 92.8805 -194.4000) (list 114.7959 -194.4000) (list 139.0297 -194.4000) (list 157.4893 -194.4000) (list 175.5000 -194.4000) (list 194.1835 -194.4000) (list 212.6432 -194.4000) (list 224.3493 -194.4000) (list 245.0601 -194.4000) (list 263.9700 -194.4000) ))
	
	(setq gjb_list23 (list  (list 92.8805 -200.9000) (list 114.7959 -200.9000) (list 140.3804 -200.9000) (list 157.4893 -200.9000) (list 175.5000 -200.9000) (list 194.1835 -200.9000) (list 212.6432 -200.9000) (list 224.3493 -200.9000) (list 245.0601 -200.9000) (list 265.0956 -200.9000) ))
	
	(setq gjb_list24 (list  (list 92.8805 -207.5000) (list 114.7959 -207.5000) (list 139.0297 -207.5000) (list 157.4893 -207.5000) (list 175.5000 -207.5000) (list 194.1835 -207.5000) (list 212.6432 -207.5000) (list 225.4749 -207.5000) (list 245.0601 -207.5000) (list 265.0956 -207.5000) ))
	
	(setq gjb_list25 (list  (list 92.8805 -218.3000) (list 116.1180 -214.5206) (list 125.7617 -218.3000) (list 139.0297 -218.3000) (list 157.4893 -218.3000) (list 175.5000 -218.3000) (list 194.1835 -218.3000) (list 212.6432 -218.3000) (list 224.3493 -218.3000) (list 245.0601 -218.3000) (list 265.0956 -218.3000) ))
	
	(setq gjb_list26 (list  (list 92.8805 -227.8000) (list 114.7959 -227.8000) (list 140.3804 -227.8000) (list 157.4893 -227.8000) (list 175.5000 -227.8000) (list 194.1835 -227.8000) (list 212.6432 -227.8000) (list 223.4488 -227.8000) (list 245.0601 -227.8000) (list 265.0956 -227.8000) ))
	
	(setq gjb_list27 (list  (list 92.8805 -234.2000) (list 114.7959 -234.2000) (list 139.0297 -234.2000) (list 157.4893 -234.2000) (list 175.5000 -234.2000) (list 194.1835 -234.2000) (list 212.6432 -234.2000) (list 224.3493 -234.2000) (list 245.0601 -234.2000) (list 263.9700 -234.2000) ))
	
	(setq gjb_list28 (list  (list 92.8805 -240.8000) (list 114.7959 -240.8000) (list 140.3804 -240.8000) (list 157.4893 -240.8000) (list 175.5000 -240.8000) (list 194.1835 -240.8000) (list 212.6432 -240.8000) (list 223.4488 -240.8000) (list 245.0601 -240.8000) (list 265.0956 -240.8000) ))
	
	(setq gjb_list29 (list  (list 92.8805 -247.3000) (list 114.7959 -247.3000) (list 139.0297 -247.3000) (list 157.4893 -247.3000) (list 175.5000 -247.3000) (list 194.1835 -247.3000) (list 212.6432 -247.3000) (list 224.3493 -247.3000) (list 245.0601 -247.3000) (list 265.0956 -247.3000) ))
	
	(setq gjb_list30 (list  (list 92.8805 -253.8000) (list 114.7959 -253.8000) (list 139.0297 -253.8000) (list 157.4893 -253.8000) (list 175.5000 -253.8000) (list 194.1835 -253.8000) (list 212.6432 -253.8000) (list 224.3493 -253.8000) (list 245.0601 -253.8000) (list 265.0956 -253.8000) ))
	
	(setq gjb_list31 (list  (list 92.8805 -260.4000) (list 114.7959 -260.4000) (list 140.3804 -260.4000) (list 157.4893 -260.4000) (list 175.5000 -260.4000) (list 194.1835 -260.4000) (list 212.6432 -260.4000) (list 224.3493 -260.4000) (list 245.0601 -260.4000) (list 265.0956 -260.4000) ))
	
	(setq gjb_list32 (list  (list 92.8805 -266.9000) (list 114.7959 -266.9000) (list 139.0297 -266.9000) (list 157.4893 -266.9000) (list 175.5000 -266.9000) (list 194.1835 -266.9000) (list 212.6432 -266.9000) (list 224.3493 -266.9000) (list 245.0601 -266.9000) (list 265.0956 -266.9000) ))
	
	(setq gjb_list33 (list  (list 92.8805 -273.0000) (list 114.7959 -273.0000) (list 139.0297 -273.0000) (list 157.4893 -273.0000) (list 175.5000 -273.0000) (list 194.1835 -273.0000) (list 212.6432 -273.0000) (list 224.3493 -273.0000) (list 245.0601 -273.0000) (list 265.0956 -273.0000) ))
	
	(setq gjb_list34 (list  (list 92.8805 -279.3000) (list 114.7959 -279.3000) (list 139.0297 -279.3000) (list 157.4893 -279.3000) (list 175.5000 -279.3000) (list 194.1835 -279.3000) (list 212.6432 -279.3000) (list 224.3493 -279.3000) (list 245.0601 -279.3000) (list 263.9700 -279.3000) ))
	
	(setq gjb_list35 (list  (list 92.8805 -289.5000) (list 115.6112 -289.5000) (list 125.7617 -289.5000) (list 139.0297 -289.5000) (list 157.4893 -289.5000) (list 175.5000 -289.5000) (list 194.1835 -289.5000) (list 212.6432 -289.5000) (list 223.4488 -289.5000) (list 245.0601 -289.5000) (list 263.9700 -289.5000) ))
	
	(setq gjb_list36 (list  (list 92.8805 -299.4000) (list 114.7959 -299.4000) (list 140.3804 -299.4000) (list 157.4893 -299.4000) (list 175.5000 -299.4000) (list 194.1835 -299.4000) (list 212.6432 -299.4000) (list 224.3493 -299.4000) (list 245.0601 -299.4000) (list 263.9700 -299.4000) ))
	
	(setq gjb_list37 (list  (list 92.8805 -305.9000) (list 114.7959 -305.9000) (list 139.0297 -305.9000) (list 157.4893 -305.9000) (list 175.5000 -305.9000) (list 194.1835 -305.9000) (list 212.6432 -305.9000) (list 224.3493 -305.9000) (list 245.0601 -305.9000) (list 263.9700 -305.9000) ))
	
	(setq gjb_list38 (list  (list 92.8805 -312.4000) (list 114.7959 -312.4000) (list 140.3804 -312.4000) (list 157.4893 -312.4000) (list 175.5000 -312.4000) (list 194.1835 -312.4000) (list 212.6432 -312.4000) (list 224.3493 -312.4000) (list 245.0601 -312.4000) (list 263.9700 -312.4000) ))
	
	(setq gjb_list39 (list  (list 92.8805 -319.4000) (list 114.7959 -319.4000) (list 139.0297 -319.4000) (list 157.4893 -319.4000) (list 175.5000 -319.4000) (list 194.1835 -319.4000) (list 212.6432 -319.4000) (list 225.4749 -319.4000) (list 245.0601 -319.4000) (list 265.0956 -319.4000) ))
	
	(setq gjb_list40 (list  (list 92.8805 -325.9000) (list 114.7959 -325.9000) (list 140.3804 -325.9000) (list 157.4893 -325.9000) (list 175.5000 -325.9000) (list 194.1835 -325.9000) (list 212.6432 -325.9000) (list 225.4749 -325.9000) (list 245.0601 -325.9000) (list 265.0956 -325.9000) ))
	
	(setq gjb_list41 (list  (list 92.8805 -336.4000) (list 105.6330 -336.4000) (list 115.6965 -336.4000) (list 139.0297 -336.4000) (list 157.4893 -336.4000) (list 175.5000 -336.4000) (list 194.1835 -336.4000) (list 212.6432 -336.4000) (list 225.4749 -336.4000) (list 245.0601 -336.4000) (list 265.0956 -336.4000) ))
	
	(setq gjb_list42 (list  (list 92.8805 -346.0000) (list 114.7959 -346.0000) (list 140.3804 -346.0000) (list 157.4893 -346.0000) (list 175.5000 -346.0000) (list 194.1835 -346.0000) (list 213.7688 -346.0000) (list 226.6005 -346.0000) (list 245.0601 -346.0000) (list 266.2212 -346.0000) ))
	
	(setq gjb_list43 (list  (list 92.8805 -352.7000) (list 114.7959 -352.7000) (list 139.0297 -352.7000) (list 157.4893 -352.7000) (list 175.5000 -352.7000) (list 194.1835 -352.7000) (list 212.6432 -352.7000) (list 225.4749 -352.7000) (list 245.0601 -352.7000) (list 265.0956 -352.7000) ))
	
	(setq gjb_list44 (list  (list 92.8805 -359.3000) (list 114.7959 -359.3000) (list 139.0297 -359.3000) (list 157.4893 -359.3000) (list 175.5000 -359.3000) (list 194.1835 -359.3000) (list 212.6432 -359.3000) (list 224.3493 -359.3000) (list 245.0601 -359.3000) (list 265.0956 -359.3000) ))
	
	(setq gjb_list45 (list  (list 92.8805 -365.8000) (list 114.7959 -365.8000) (list 140.3804 -365.8000) (list 157.4893 -365.8000) (list 175.5000 -365.8000) (list 194.1835 -365.8000) (list 212.6432 -365.8000) (list 224.3493 -365.8000) (list 245.0601 -365.8000) (list 265.0956 -365.8000) ))
	
	(setq gjb_list46 (list  (list 92.8805 -375.8000) (list 105.7667 -375.8000) (list 114.9127 -377.5351) (list 139.0297 -375.8000) (list 157.4893 -375.8000) (list 175.5000 -375.8000) (list 194.1835 -375.8000) (list 212.6432 -375.8000) (list 224.3493 -375.8000) (list 245.0601 -375.8000) (list 265.0956 -375.8000) ))
	
	(setq gjb_list47 (list  (list 92.8805 -384.2000) (list 114.7959 -384.2000) (list 140.3804 -384.2000) (list 157.4893 -384.2000) (list 175.5000 -384.2000) (list 194.1835 -384.2000) (list 212.6432 -384.2000) (list 225.4749 -384.2000) (list 245.0601 -384.2000) (list 265.0956 -384.2000) ))
	
)
(defun gj_bh( bianhao zhijing)
	(setq linpd0 (getvar "lastpoint"))
	(command "text" "j" "mL" linpd0 50 0 (strcat (itoa bianhao) "  %%130 " (itoa zhijing) " @200"));3号筋
	(command "CIRCLE" (polar linpd0 0 15) 40);画编号 的圆，字体样式
	
)	

(defun gj_h1 ( pt1 pt2 pt3 pt4 / lls ptx pty  linls linls1);pt1 pt2 的方向一定要，1是下，2 是上,pt3 is down,pt4 is 
	;画竖向钢筋，同时左右两个，斜向在左
	
	(setq lls (abs (distance pt1 pt2)))
	(setq gshu (+ (fix (+ (/ lls jjgj) 1.00)) 1)) ;钢筋根数gshu应该按断面的尺寸计算，因为配筋是按
	
	(setq lina_gj1 (angle pt1 pt2));1 angle
	(setq lina_gj2 (angle pt3 pt4));2 angle
	(if (= lina_gj1 0) (progn
											 (setq linls1 ls)
											 (setq linlsgjd1 ls1)
										 )
		(progn
			(setq linls1 (/ ls (sin lina_gj1)))
			(setq linlsgjd1 (/ ls1 (sin lina_gj1)))
			(setq hx1 (/ hbaohu (/ (sin lina_gj1) (cos lina_gj1))));不同角度下左右两侧数不一样。
		);直角的时候判断一下，以前是用实数有很多小数点来，不精确
	);钢筋点的距离
	
	(if (= lina_gj2 0) (progn
											 (setq linls2 ls)
											 (setq linlsgjd2 ls1)
											 (setq hx1 0);不同角度下左右两侧数不一样。
											 (setq hx2 0		 ))
		(progn
			(setq linls2 (/ ls (sin lina_gj2)))
			(setq linlsgjd2 (/ ls1 (sin lina_gj2)))
			
			(setq hx2 (/ hbaohu (/ (sin lina_gj2) (cos lina_gj2))))			 );
	);钢筋点的距(if (= lina_gj2 0) (setq linls2 ls) (setq linls2 (/ ls (sin lina_gj2))));直角的时候判断一下，以前是用实数有很多小数点来，不精确
	
	
	(setq ptx1 (+ linls1 hx1))
	(setq pty1 hbaohu)
	(setq ptx2 (- linls2 hx2))
	(setq pty2 hbaohu)
	(setq lingjx1-1 (zbjs pt1 ptx1 pty1))	;(第一条钢筋线的基点坐标
	
	(setq lingjx1-2 (zbjs pt2  ptx1  (* -1 pty1)))
	(setq lingjx2-1 (zbjs pt3 (* -1 ptx2) pty2))
	(setq lingjx2-2 (zbjs pt4 (* -1 ptx2) (* -1 pty2)))
	(setq lingjd1-1 (zbjs pt1 (+ linlsgjd1 hx1) pty1));点坐标
	(setq lingjd1-2 (zbjs pt2 (- linlsgjd1 hx1) (* -1 pty1)))
	(setq lingjd2-1 (zbjs pt3 (* -1 (+ linlsgjd2 hx2)) pty2))
	(setq lingjd2-2 (zbjs pt4 (* -1 (+ linlsgjd2 hx2)) (* -1 pty2)))
	
	(setq jjgs1 (/ (abs (distance lingjd1-1 lingjd1-2)) (- gshu 1)))	;应该实际调整间距。
	(setq jjgs2 (/ (abs (distance lingjd2-1 lingjd2-2)) (- gshu 1)))	;应该实际调整间距。
	(command "line" lingjx1-1 lingjx1-2 "")
	(command "line" lingjx2-1 lingjx2-2 "")
	
	(setq lingjd-1 lingjd1-1
		lingjd-2 lingjd2-1
		lingjdzd (zd pt1 pt3);		
	)
	
	(repeat  gshu 
		(setq lingjdzd0 lingjdzd);保留前一次的值 ，为了画连接结用
		(command "donut" 0 rgj lingjd-1 "");draw right point 
		(command "donut" 0 rgj lingjd-2 "");draw left point 
		(command "pline" lingjd-1 lingjdzd lingjd-2 "")
		(setq lingjdzd (zd lingjd-1 lingjd-2))
		(setq lingjd-1 (polar lingjd-1 lina_gj1 jjgs1))
		(setq lingjd-2 (polar lingjd-2 lina_gj2  jjgs2)	)
	)
	(command "pline" lingjdzd0 (zd pt1 pt3)  "")
	
)

(defun gj_h2 ( pt1 pt2 pt3 pt4 / lls ptx pty  ptxd ptyd linls linls1);横向 钢筋 。pt1 pt2 是下面一根筋 。的方向一定要，1是下，2 是上,pt3 is down,pt4 is 
	
	(setq lls (abs (distance pt1 pt2)))
	(setq gshu (+ (fix (+ (/ lls jjgj) 1)) 1)) ;钢筋根数gshu应该按断面的尺寸计算，因为配筋是按
	(setq ptx hbaohu )
	(setq pty ls)
	(setq ptx1 (+ hbaohu (/ rgj 2)) )
	(setq pty1 ls1)
	
	(setq lingjx1-1 (zbjs pt1 ptx pty))	;(第一条钢筋线的基点坐标
	(setq lingjx2-1 (zbjs pt3 ptx (* pty -1)))
	(setq lingjx1-2 (zbjs pt2 (* -1 ptx) pty))
	(setq lingjx2-2 (zbjs pt4  (* -1 ptx) (* pty -1)))
	
	
	(setq lingjd1-1 (zbjs pt1 ptx1 pty1));点坐标
	(setq lingjd2-1 (zbjs pt3 ptx1 (* pty1 -1)))
	
	(setq lingjd1-2 (zbjs pt2 (* -1 ptx1) pty1))
	(setq lingjd2-2 (zbjs pt4  (* -1 ptx1) (* pty1 -1)))
	
	(setq jjgs1 (/ (abs (distance lingjd1-1 lingjd1-2)) (- gshu 1)))	;应该实际调整间距。
	(command "line" lingjx1-1 lingjx1-2 "")
	(command "line" lingjx2-1 lingjx2-2 "")
	
	(setq lingjd-1 lingjd1-2
		lingjd-2 lingjd2-2
		lingjdzd (zd lingjd1-2 lingjd2-2);		
	)
	
	(repeat  gshu 
		(command "pline" lingjd-1 lingjdzd lingjd-2 "")
		(setq lingjdzd0 lingjdzd);保留前一次的值 ，为了画连接结用
		(command "donut" 0 rgj lingjd-1 "");draw right point 
		(command "donut" 0 rgj lingjd-2 "");draw left point 
		(setq lingjdzd (zd lingjd-1 lingjd-2))
		(setq lingjd-1 (polar lingjd-1 0 (* -1 jjgs1)))
		(setq lingjd-2 (polar lingjd-2 0 (* -1 jjgs1)))	
		
	)
	(command "pline" lingjdzd0 (zd pt2 pt4)  "")
	
)

