<template>
    <div>
        <head-top></head-top>
		<div class="fruit-content">
		<el-row style="margin-top: 20px;">
            <el-col :span="2" style="text-align:right;">商品编码：</el-col>
			<el-col :span="4"><el-input v-model="repocode" siez="mini" placeholder="请输入内容"></el-input></el-col>
            <el-col :span="2" style="text-align:right;">商品名称：</el-col>
			<el-col :span="4"><el-input v-model="proname" siez="mini" placeholder="请输入内容"></el-input></el-col>
            <el-col :span="2" style="text-align:right;">仓库名称：</el-col>
			<el-col :span="4"><el-input v-model="repositorystate" siez="mini" placeholder="请输入内容"></el-input></el-col>
		</el-row>
		<el-row>
			<el-col :span="24"><el-button style="float: right;" @click="handleSearch" type="primary">查询</el-button></el-col>
		</el-row>
        <el-table
            :data="receiptData"
            stripe
            style="text-align:left;">
            <el-table-column
            prop="repocode"
            label="商品编码">
            </el-table-column>
            <el-table-column
            prop="proname"
            label="商品名称">
            </el-table-column>
            <el-table-column
            prop="goodstype"
            label="商品分类">
            </el-table-column>
            <el-table-column
            prop="prostandard"
            label="规格型号">
            </el-table-column>
            <el-table-column
            prop="prounite"
            label="库存单位">
            </el-table-column>
            <el-table-column
            prop="repositorystate"
            label="仓库名称">
            </el-table-column>
            <el-table-column
            prop="currentexist"
            label="库存单位现存量">
            </el-table-column>
        </el-table>
        <div class="Pagination" style="text-align: left;margin-top: 10px;">
			<el-pagination
				@size-change="handleSizeChange"
				@current-change="handleCurrentChange"
				:current-page="currentPage"
				:page-size="20"
				layout="total, prev, pager, next"
				:total="count">
			</el-pagination>
		</div>
		</div>
    </div>
</template>

<script>
    import headTop from '@/components/headTop'
    import {getStockInListAll, queryStockInList, getBalanceTableB} from '@/api/getData'
    import {baseUrl, baseImgPath} from '@/config/env'
    export default {
    	data(){
    		return {
				value1: '',
				value2: '',
				input: '',
				city: {},
				receiptData: [],
                currentPage: 1,
                count: 0,
                proname: '',
                repocode: '',
                repositorystate: ''
    		}
    	},
    	components: {
    		headTop,
    	},
    	mounted(){
    		this.initData();
    	},
    	methods: {
    		async initData(){
    			try{
					const dataReceipt = await getStockBalancebAll()
					console.log('re: ',dataReceipt.data.data)
					if(dataReceipt.data.code === '1111'){
						this.receiptData = dataReceipt.data.data.list
						this.count = dataReceipt.data.data.total
					}
    			}catch(err){
    				console.log(err);
    			}
    		},
			handleEdit(index,row) {
				console.log(index,row)
				this.$destroy()
				this.$router.push('/stockInListDetails/'+ row.orderid)
			},
			async handleSearch(){
                let {repocode, proname, repositorystate} = this
				const dataReceipt = await getBalanceTableB(null, null, repocode, proname, repositorystate)
                this.receiptData = dataReceipt.data.data.list
                this.count = dataReceipt.data.data.total
			},
			formatter(date){
				console.log(date.getMonth())
				let res = ''
				res += date.getFullYear()+ '-' + (date.getMonth() + 1) + '-' +date.getDate()
				return res
			},
            async handleCurrentChange(num){
                const dataReceipt = await getBalanceTableB(num)
                this.receiptData = dataReceipt.data.data.list
            },
            handleSizeChange(...rest) {
                console.log('rest', rest)
            }
		}
    }
</script>

<style lang="less" scoped>
	@import '../style/mixin';
	.button_submit{
		text-align: center;
	}
	.avatar-uploader .el-upload {
	    border: 1px dashed #d9d9d9;
	    border-radius: 6px;
	    cursor: pointer;
	    position: relative;
	    overflow: hidden;
	}
	.avatar-uploader .el-upload:hover {
	    border-color: #20a0ff;
	}
	.avatar-uploader-icon {
	    font-size: 28px;
	    color: #8c939d;
	    width: 120px;
	    height: 120px;
	    line-height: 120px;
	    text-align: center;
	}
	.avatar {
	    width: 120px;
	    height: 120px;
	    display: block;
	}
	.el-table .info-row {
	    background: #c9e5f5;
	}

	.el-table .positive-row {
	    background: #e2f0e4;
	}
	.el-row {
		margin-bottom: 20px;
		&:last-child {
		margin-bottom: 0;
		}
	}
	.el-col {
		border-radius: 4px;
		height: 36px;
		line-height: 36px;
		font-size: 14px;
	}
	.bg-purple-dark {
		background: #99a9bf;
	}
	.bg-purple {
		background: #d3dce6;
	}
	.bg-purple-light {
		background: #e5e9f2;
	}
	.grid-content {
		border-radius: 4px;
		min-height: 36px;
	}
	.row-bg {
		padding: 10px 0;
		background-color: #f9fafc;
	}
	.el-input__inner {
		height: 32px !important;
	}
	.fruit-content {
		padding: 0 4%;
	}
</style>
