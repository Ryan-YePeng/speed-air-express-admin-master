<template>
  <el-dialog
          title="新增快递员"
          width="500px"
          @close="cancel"
          :close-on-click-modal="false"
          :visible.sync="dialogTableVisible">
    <el-form :model="form" :rules="rules" ref="Form" label-width="120px" hide-required-asterisk>
      <el-form-item label="姓名" prop="c_co_name">
        <el-input v-model="form.c_co_name"></el-input>
      </el-form-item>
      <el-form-item label="编号" prop="c_co_number">
        <el-input v-model="form.c_co_number"></el-input>
      </el-form-item>
      <el-form-item label="联系方式" prop="c_co_contact">
        <el-input v-model="form.c_co_contact"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogTableVisible = false">取 消</el-button>
      <el-button type="primary" @click="submitForm('Form')">确 定</el-button>
    </div>
  </el-dialog>
</template>

<script>
  import {addCourierApi} from '@/api/courier'

  export default {
    name: "AddCourier",
    data() {
      return {
        dialogTableVisible: false,
        form: {
          c_co_name: '',
          c_co_number: '',
          c_co_contact: '',
          l_co_name: '',
          l_co_number: '',
          l_co_contact: ''
        },
        rules: {
          c_co_name: {required: true, message: '请输入姓名', trigger: 'blur'},
          c_co_number: {required: true, message: '请输入编号', trigger: 'blur'},
          c_co_contact: {required: true, message: '请输入联系方式', trigger: 'blur'},
          l_co_name: {required: true, message: '请输入姓名', trigger: 'blur'},
          l_co_number: {required: true, message: '请输入编号', trigger: 'blur'},
          l_co_contact: {required: true, message: '请输入联系方式', trigger: 'blur'},
        }
      }
    },
    computed: {
      userId() {
        return this.$store.getters.userId
      }
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            let data = {...this.form};
            data.l_user_id = this.userId;
            addCourierApi(data).then(() => {
              this.$emit('update');
              this.cancel()
            });
          } else {
            return false;
          }
        });
      },
      cancel() {
        this.dialogTableVisible = false;
        Object.assign(this.$data.form, this.$options.data().form);
        this.$refs['Form'].resetFields()
      }
    }
  }
</script>

<style scoped>

</style>
