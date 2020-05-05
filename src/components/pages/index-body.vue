<template>
  <div class="index-body">
    <el-row :gutter="20" style="margin-top: 30px;">
      <el-col :span="24">
        <el-row style="margin-top: 30px;">
          <el-carousel direction="horizontal" :autoplay="true" height="370px">
            <el-carousel-item v-for="(item,index) in items" :key="item">
              <img :src="item" alt="请检查网络连接" width="100%">
            </el-carousel-item>
          </el-carousel>
        </el-row>
        <!--   //搜索-->
        <el-row style="position: relative;top:73px;">
          <el-col :span="5" :offset="16">
            <el-input
              placeholder="输入商品名、商品ID"
              v-model="search"
              clearable
              @clear="shopCarNameSearch()">
            </el-input>
          </el-col>
          <el-col :span="2">
            <el-button type="primary" icon="el-icon-search" @click="clickSearch"
                       v-loading.fullscreen.lock="fullscreenLoading">搜索
            </el-button>
          </el-col>
        </el-row>

        <p class="index-body-p-1" id="hot-text">热门精品</p>
        <hr/>
        <el-row style="margin-top: 50px;">
          <el-col :span="4" v-for="(item,index) in booksHotShowList" style="margin-left: 10px">
            <el-card :body-style="{ padding: '0px', textAlign: 'center'}">
              <img v-bind:src="item.bookImg" class="image" style="width: 100%" alt="请检查网络连接">
              <div style="padding: 10px;">
                <h4>{{item.bookPrice}}</h4>
                <h4>{{item.bookName}}</h4>
                <div class="bottom clearfix">
                  <el-button type="text" class="button" @click="clickGoodsInfoButton(item.bookId)">
                    查看详情
                  </el-button>
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>

        <!--        分页组件-->
        <el-row style="text-align: center;margin-top: 80px;" v-show="paginationShow">
          <el-col :span="24">
            <!--            限定每页 12 条数据-->
            <el-pagination
              @current-change="handleCurrentChange"
              :current-page="currentPage"
              :page-sizes="[12]"
              :page-size="pageSize"
              layout="prev, pager, next"
              :total="dataSearch.length">
            </el-pagination>
          </el-col>
        </el-row>
        <div class="index-body-p-total" id="index-body-id">
          <p class="index-body-p-1" style="margin-top: 60px;">低价好物</p>
          <hr/>
          <el-row style="margin-top: 50px;">
            <el-col :span="4" v-for="(item,index) in booksLowPriceShowList" style="margin-left: 10px">
              <el-card :body-style="{ padding: '0px', textAlign: 'center'}">
                <img v-bind:src="item.bookImg" class="image" style="width: 100%" alt="请检查网络连接">
                <div style="padding: 10px;">
                  <h4>{{item.bookPrice}}</h4>
                  <h4>{{item.bookName}}</h4>
                  <div class="bottom clearfix">
                    <el-button type="text" class="button" @click="clickGoodsInfoButton(item.bookId)">
                      查看详情
                    </el-button>
                  </div>
                </div>
              </el-card>
            </el-col>
          </el-row>

          <p class="index-body-p-1" style="margin-top: 60px;">9 新以上</p>
          <hr/>
          <el-row style="margin-top: 50px;">
            <el-col :span="4" v-for="(item,index) in booksMostNewShowList" style="margin-left: 10px">
              <el-card :body-style="{ padding: '0px', textAlign: 'center'}">
                <img v-bind:src="item.bookImg" class="image" style="width: 100%" alt="请检查网络连接">
                <div style="padding: 10px;">
                  <h4>{{item.bookPrice}}</h4>
                  <h4>{{item.bookName}}</h4>
                  <div class="bottom clearfix">
                    <el-button type="text" class="button" @click="clickGoodsInfoButton(item.bookId)">
                      查看详情
                    </el-button>
                  </div>
                </div>
              </el-card>
            </el-col>
          </el-row>
        </div>
      </el-col>
    </el-row>


    <!--      商品详情弹出框-->
    <el-dialog
      :title="hotSelect.bookName"
      :visible.sync="centerDialogVisible"
      width="418px"
      center>
      <div style="text-align: center">
        <el-carousel direction="horizontal" :autoplay="true">
          <!--          商品图片源于淘宝，淘宝采用418 * 418 的商品预览图-->
          <el-carousel-item style="width: 365px;height: 300px;">
            <img :src="hotSelect.bookImg" alt="请检查网络连接" width="93%">
          </el-carousel-item>
          <el-carousel-item style="width: 365px;height: 300px;">
            <img alt="请检查网络连接" width="80%">
          </el-carousel-item>
        </el-carousel>
        <h2><img src="https://i.loli.net/2019/12/17/NhTJjayS6ZCsHWP.png" alt="￥" width="30px">：{{ hotSelect.bookPrice }}
        </h2>
        <p>{{ hotSelect.bookDescribe }}</p>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="addToShopCar" style="margin-top: 30px;">加入购物车</el-button>
        </span>
      </div>
    </el-dialog>

    <!--      加入购物车结果提示框-->
    <el-dialog
      title="提示"
      :visible.sync="centerDialogVisible2"
      width="30%"
      center>
      <div style="text-align: center">
        <h4>{{ dialogValue }}</h4>
        <span slot="footer" class="dialog-footer">
          <el-row style="margin-top: 30px;">
            <el-col :span="6" :offset="5">
              <el-button type="primary" @click="clickButton" style="width: 100%">确 定</el-button>
            </el-col>
            <el-col :span="6" :offset="2">
              <el-button type="primary" @click="clickGoToShopCar">前往购物车</el-button>
            </el-col>
          </el-row>
        </span>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "index-body",
    data() {
      return {
        items: {
          img2: require("../../assets/school1.jpg"),
          img3: require("../../assets/school3.jpg"),
          img4: require("../../assets/school4.jpg"),
        },
        rules: {
          search: [
            {validator: checkString, trigger: 'blur'}
          ]
        },
        search: "",
        dataSearch: [],//用来保存搜索的全部结果
        booksInfoId: "",
        centerDialogVisible: false,
        dialogValue: "",
        centerDialogVisible2: false,
        currentPage: 1, //初始页
        pageSize: 12, //每页的数据
        paginationShow: false,//默认不显示分页
        searchFlag: false,//用来避免频繁向服务器发送数据
        fullscreenLoading: false,//模拟加载

        //
        booksList: [],
        booksHotShowList: [],//热门精品
        booksLowPriceShowList: [],//低价好物
        booksMostNewShowList: [],//九新以上
        bookSelectedIndex: -1,
        hotSelect: "",

      };
      let checkString = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('请填写内容'));
        } else {
          callback();
        }
      };
    },
    mounted() {
      this.allBooks();
      let self = this;
      //延迟 0.3 S 绑定数据
      self.fullscreenLoading = true;
      setTimeout(() => {
        self.fullscreenLoading = false;
      }, 300);
    },
    methods: {
      //booksTable
      allBooks() {
        let self = this;
        self.booksList = [];
        self.booksLowPriceShowList = [];
        self.booksHotShowList = [];
        self.booksMostNewShowList = [];
        this.$db.transaction(function (tx) {
          tx.executeSql('SELECT * FROM BOOKS', [], function (tx, results) {
            let len = results.rows.length, i;
            for (i = 0; i < len; i++) {//把查出来的数据封装到一个对象里面 最后放到数组里面
              let BookId = results.rows.item(i).bookId;
              let BookImg = results.rows.item(i).bookImg;
              let BookName = results.rows.item(i).bookName;
              let BookPrice = results.rows.item(i).bookPrice;
              let BookDescribe = results.rows.item(i).bookDescribe;
              let BookCount = results.rows.item(i).bookCount;
              let Type = results.rows.item(i).type;
              let book = {};//创建一个js对象
              book.bookId = BookId;
              book.bookImg = BookImg;
              book.bookName = BookName;
              book.bookPrice = BookPrice;
              book.bookDescribe = BookDescribe;
              book.bookCount = BookCount;
              book.type = Type;
              self.booksList.push(book)
            }
            for (let data of self.booksList) {
              if (data.type === 1) {
                self.booksHotShowList.push(data);
              } else if (data.type === 2) {
                self.booksLowPriceShowList.push(data);
              } else if (data.type === 3) {
                self.booksMostNewShowList.push(data);
              }
            }

          });//将数组赋值给vue创建的数组
          return self.booksList;

        })

      },
      //点击查看详情
      clickGoodsInfoButton(bookId) {
        let self = this;
        this.centerDialogVisible = true;
        // 显示点击的查看详情的信息
        this.$db.transaction(function (tx) {
          //根据id查到的数据永远只有一个，id是唯一的
          tx.executeSql('SELECT * FROM BOOKS where bookId = ?', [bookId], function (tx, results) {
            self.hotSelect = results.rows[0];
            if (results.rows.length === 0) {
              this.centerDialogVisible = true;
              this.dialogValue = "未找到此书！";
            }
          })
        })
      },
      //点击加入购物车按钮
      addToShopCar() {
        //如果没登录，提示登录
        if (this.userName === "登录/注册") {
          this.centerDialogVisible = true;
          this.dialogValue = "您还未登录，请先登录！"
        } else {
          this.centerDialogVisible2 = true;
        }
      },
      //购物车确定
      //点击购物车确定时，往购物车数据库插入一条数据
      clickButton() {
        this.centerDialogVisible = false;
        this.centerDialogVisible2 = false;
        let self = this;
        this.$db.transaction(function (tx) {
            // INSERT INTO USER (account, password,nickName,mail) VALUES (?, ?, ?, ?)', [self.inputUsername, self.inputPassword, self.inputName, self.inputEmail], function ()
            tx.executeSql('INSERT INTO SHOPCAR (shopCarId ,shopCarImg,shopCarPrice,shopCarName)VALUES(?,?,?,?)', [self.hotSelect.bookId, self.hotSelect.bookImg, self.hotSelect.bookPrice, self.hotSelect.bookName], function (tx, results) {
              console.log("加入购物车成功")
            })
          }
        )

      },
      //点击跳转购物车按钮
      //查询显示所有购物车数据库的数据
      clickGoToShopCar() {
        this.clickButton();
        this.$router.push('shopCar');
      }
      ,
      // 初始页currentPage、初始每页数据数pageSize和数据data
      // 点击了分页组件换页按钮
      handleCurrentChange(currentPage) {
        this.currentPage = currentPage;
        // console.log(this.currentPage)  //点击第几页
        //切割 dataSearch 赋值给 data1
        // this.data1 = this.dataSearch.slice((this.currentPage-1)*12,this.currentPage*12);
        this.data1 = this.mySlice(this.dataSearch, (this.currentPage - 1) * 12, this.currentPage * 12);
      }
      ,
      //点击搜索按钮
      //低价好物和9新以上隐藏，
      clickSearch() {
        if (this.search !== "") {
          let self = this;
          //延迟 0.3 S 绑定数据
          self.fullscreenLoading = true;
          setTimeout(() => {
            self.fullscreenLoading = false;
          }, 300);
          //动态添加属性
          document.getElementById("index-body-id").setAttribute("style", "display: none;");
          document.getElementById("hot-text").innerText = "全部";
          this.$db.transaction(function (tx) {
              tx.executeSql("SELECT * FROM BOOKS where bookName like ? ", ['%' + self.search + '%'], function (tx, results) {
                self.booksHotShowList = [];
                console.log(results.rows);
                self.booksHotShowList = results.rows;
              }, function () {
                console.log("11")
              })
            }
          )
        }
      },
      //点击xx，清除搜索
      shopCarNameSearch() {
        document.getElementById("index-body-id").setAttribute("style", "display:block;");
        document.getElementById("hot-text").innerText = "热门精品";
        this.allBooks()
      },
    }
  }
</script>

<style scoped>
  .index-body {
    width: 80%;
    margin: 0 auto;
  }

  .index-body-p-1 {
    font-weight: bold;
    font-size: 30px;
    color: black;
    margin-top: 30px;
  }


</style>
