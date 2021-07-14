<template>
  <a-form :form="form" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }" @submit="handleSubmit">
    <a-form-item label="网站名称">
      <a-input v-decorator="['note', { rules: [{ required: true, message: '请输入网站名称!' }] }]"/>
    </a-form-item>
    <a-form-item label="网站链接">
      <a-input default-value="" v-decorator="['link',
                                { rules: [{ required: true, message: '请输入网站链接!' }] }]">
        <a-select slot="addonBefore" default-value="Http://" style="width: 90px">
          <a-select-option value="Http://"> Http:// </a-select-option>
          <a-select-option value="Https://">Https:// </a-select-option>
        </a-select>
      </a-input>
    </a-form-item>
  <!--  网站类型  -->
    <a-form-item label="网站类型">
    <a-cascader
        :options=options
        :display-render="displayRender"
        :allowClear = true
        expand-trigger="hover"
        placeholder="Please select"
        @change="onChange"
    />
    </a-form-item>
<!--  是否可用   -->
    <a-form-item label="是否可用">
      <a-switch checked-children="可用" un-checked-children="不可用" default-checked />
    </a-form-item>
<!--   星级评分 -->
    <a-form-item label="网站评级">
      <a-rate :default-value="5.0" allow-half />
    </a-form-item>
<!--  评分  -->
    <a-form-item label="评分">
      <a-input-number :min="0" :max="100" :disabled="false" :default-value="3" />
    </a-form-item>
    <a-form-item label="网站简介">
      <a-textarea placeholder="Basic usage" :rows="4" />
    </a-form-item>
    <a-form-item :wrapper-col="{ span: 12, offset: 5 }">
      <a-button type="primary" html-type="submit">
        提交
      </a-button>
    </a-form-item>
  </a-form>
</template>


<script>
const siteType =  [
  {
    value: 'zhejiang',
    label: 'Zhejiang',
    children: [
      {
        value: 'hangzhou',
        label: 'Hangzhou',
        children: [
          {
            value: 'xihu',
            label: 'West Lake',
          },
        ],
      },
    ],
  },
  {
    value: 'jiangsu',
    label: 'Jiangsu',
    children: [
      {
        value: 'nanjing',
        label: 'Nanjing',
        children: [
          {
            value: 'zhonghuamen',
            label: 'Zhong Hua Men',
          },
        ],
      },
    ],
  },
];
export default {
  data() {
    return {
      options: siteType,
      formLayout: 'horizontal',
      form: this.$form.createForm(this, { name: 'coordinated' }),
    };
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values);
        }
      });
    }
  },
};
</script>