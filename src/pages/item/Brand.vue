<template>
<v-card>
    <v-card-title>
         <v-btn class="ma-2" outlined small fab color="primary" @click="add">
        <v-icon >add</v-icon>
    </v-btn>
    <v-spacer/>
    <v-text-field
            label="请输入搜索条件"
            append-icon="search"
            v-model="search"
            single-line
            hide-details
            class="flex sm3"
          />
    </v-card-title>
    
    <v-divider/>
     <v-data-table
    :headers="headers"
    :items="brands"
    :items-per-page="5"
    :pagination.sync="pagination"
    :total-items="totalBrands"
    :loading="loading"
    class="elevation-1"
  >
  <template  slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image" /></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <div class="justify-center layout px-0">
             <v-btn icon class="mx-0" @click="editItem(props.item)">
                  <v-icon color="success">edit</v-icon>
              </v-btn>
              <v-btn icon class="mx-0" @click="delItem(props.item)">
                  <v-icon color="warning">delete</v-icon>
              </v-btn>
        </div>
       
  </template>
  </v-data-table>
  <v-dialog max-width="600" scrollable v-model="show" persistent >
    <v-card>
        <!--对话框的标题-->
        <v-toolbar dense dark color="primary">
            <span class="headline">新增品牌</span>
            <v-spacer/>
            <v-btn icon @click="closeWindow" >
                <v-icon>close</v-icon>
            </v-btn>
        </v-toolbar>
        <!--对话框的内容，表单-->
        <v-card-text class="px-5" style="height: 600px;">
           <BrandForm></BrandForm>
        </v-card-text>
    </v-card>
</v-dialog>
</v-card>

</template>

<script>
import BrandForm from './BranForm'
export default {
    name: "brand",
    data(){
        return {
            headers:[
                {text:"id",value:"id",align:"center",sortable:true},
                {text:"名称",value:"name",align:"center",sortable:false},
                {text:"LOGO",value:"image",align:"center",sortable:false},
                {text:"首字母",value:"letter",align:"center",sortable:false},
                 {text:"操作",value:"action",align:"center",sortable:false},
            ],
            brands:[],
            pagination:{ },
            totalBrands:0,
            loading:false,
            search:'',
            show:false,

        }
    },
    watch:{
        pagination:{
            deep:true,
            handler(){
                this.getData();
            }
        },
        search:{
            deep:true,
            handler(){
                this.getData();
            }
        }
    },
    created(){
        this.getData();
    },
    methods:{
        add(){
            this.show=true;
        },
        closeWindow(){
            // 关闭窗口
            this.show = false;
        },
        getData(){
            this.loading=true;
            this.$http.get("/item/brand/page",{
                params:{
                    key: this.search, // 搜索条件
                    page: this.pagination.page,// 当前页
                    rows: this.pagination.rowsPerPage,// 每页大小
                    sortBy: this.pagination.sortBy,// 排序字段
                    desc: this.pagination.descending// 是否降序
                }
            }).then(resp=>{
                this.brands=resp.data.items,
                this.totalBrands=resp.data.total,
                this.loading=false;
            })
        },
        editItem(item){
            console.log(item)
        },
         delItem(item){
             this.$http.delete("/item/brand",JSON.stringify(item)).then(resp=>{
                this.$message.info("删除成功！");
               this.getData();
            })
        },
    },
    components:{
        BrandForm
    }
}
</script>

<style>

</style>