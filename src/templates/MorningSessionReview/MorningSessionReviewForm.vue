<script setup lang="ts">
import { themeMap } from "./theme/index";
import type { UploadProps, UploadFile } from 'element-plus';
import * as XLSX from 'xlsx';

export type MorningSessionReviewForm = {
  /** 主题 */
  theme: keyof typeof themeMap;
  /** 上传的Excel文件 */
  excelData: any[];
};

export type MorningSessionReviewFormExport = {
  theme: MorningSessionReviewForm["theme"];
  excelData: MorningSessionReviewForm['excelData'];
};

/** Props */
const props = defineProps({
  default: {
    type: Object as () => MorningSessionReviewForm,
    default: () => ({}),
  },
});

/** Emits */
const emits = defineEmits<{
  (e: "change", value: MorningSessionReviewFormExport): void;
}>();

/** Static */
const themeOption = [
  {
    label: "红色主题",
    value: "red",
  },
  {
    label: "紫色主题",
    value: "purple",
  },
  {
    label: "棕色主题",
    value: "brown",
  },
  {
    label: "绿色主题",
    value: "green",
  },
  {
    label: "蓝色主题",
    value: "blue",
  },
  {
    label: "深蓝主题",
    value: "darkblue",
  },
];

/** Ref */
const form = ref<MorningSessionReviewForm>({
  theme: props.default?.theme || 'red',
  excelData: props.default?.excelData || []
});

/** Method */
const handleFileUpload: UploadProps['onChange'] = (uploadFile: UploadFile) => {
  const file = uploadFile.raw;
  if (!file) return;
  
  const reader = new FileReader();
  reader.onload = (e) => {
    const data = new Uint8Array(e.target?.result as ArrayBuffer);
    const workbook = XLSX.read(data, { type: 'array' });
    const sheetName = workbook.SheetNames[0];
    const worksheet = workbook.Sheets[sheetName];
    const jsonData = XLSX.utils.sheet_to_json(worksheet);
    
    console.log('Excel解析结果:', jsonData);
    console.log('第一行数据示例:', jsonData[0]);
    
    form.value.excelData = jsonData;
  };
  reader.readAsArrayBuffer(file);
};

const handleThemeChanged = (val: string) => {
  // 主题变更逻辑保留，但移除orderBy设置
  console.log('主题已切换至:', val);
};

/** Watch */
watch(
  () => props.default,
  (val) => {
    const result = val;
    form.value = result;
  }
);

watch(
  () => form.value,
  (val) => {
    emits("change", val);
  },
  {
    deep: true,
  }
);
</script>

<template>
  <div class="morning-session-review-form">
    <ElForm :model="form" label-position="top">
      <ElFormItem label="选择主题">
        <ElSelect v-model="form.theme" @change="handleThemeChanged" placeholder="请选择主题">
          <ElOption
            v-for="item in themeOption"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </ElSelect>
      </ElFormItem>
      
      <ElFormItem label="上传Excel文件">
        <ElUpload
          :auto-upload="false"
          :show-file-list="true"
          :limit="1"
          accept=".xlsx,.xls"
          @change="handleFileUpload"
        >
          <ElButton type="primary">选择Excel文件</ElButton>
          <template #tip>
            <div class="el-upload__tip">
              请上传包含期货品种、数据涨幅、资金流向、沉淀资金、所属板块等字段的Excel文件
            </div>
          </template>
        </ElUpload>
      </ElFormItem>
    </ElForm>
  </div>
</template>

<style lang="less" scoped>
.morning-session-review-form {
    width: 100%;
    height: 100%;
}
</style>