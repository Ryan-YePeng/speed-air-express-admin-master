<template>
  <el-dialog
          title="编辑电话"
          width="450px"
          @close="cancel"
          :close-on-click-modal="false"
          :visible.sync="dialogTableVisible">
    <el-form :model="form" :rules="rules" ref="Form" label-width="100px" hide-required-asterisk>
      <el-form-item label="电话" prop="c_s_phone">
        <el-input v-model="form.c_s_phone"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogTableVisible = false">取 消</el-button>
      <el-button type="primary" @click="submitForm('Form')">确 定</el-button>
    </div>
  </el-dialog>
</template>

<script>
  import {updatePhoneApi} from '@/api/help'

  export default {
    name: "EditPhone",
    data() {
      return {
        dialogTableVisible: false,
        form: {
          l_s_id: 0,
          l_s_phone: '',
          c_s_phone: ''
        },
        rules: {
          l_s_phone: {required: true, message: '请输入电话', trigger: 'blur'},
          c_s_phone: {required: true, message: '请输入电话', trigger: 'blur'},
        }
      }
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            updatePhoneApi(this.form).then(() => {
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
