<script setup>
import { ref, computed } from 'vue'
import useClipboard from 'vue-clipboard3'
import { ElMessage } from 'element-plus'
const textarea2 = ref('')

// 计算属性，将包含\n的数据处理后返回
const formattedData = computed(() => {
  if (textarea2.value.includes('\n')) {
    const lines = textarea2.value.split('\n');
    return lines.filter(line => line.trim() !== '') // 过滤掉空行
      .map((line, index, arr) => {
        return `'${line}${index === arr.length - 1 ? "'" : "',"}`;
      });
  } else {
    return [];
  }

});

const clickCopy = async () => {
  const { toClipboard } = useClipboard()
  console.log(formattedData.value.join('\n').trimEnd())
  try {
    await toClipboard(formattedData.value.join('\n').trimEnd())
    console.log('Copied to clipboard')
    ElMessage({
      message: '复制成功',
      type: 'success',
    })
  } catch (e) {
    console.error(e)
    ElMessage.error('复制失败')
  }

}



</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <div class="wrapper">
      <div class="greetings">
        <h1 class="green">LíJimpéy(：</h1>
        <el-input v-model="textarea2" style="width: 240px" :autosize="{ minRows: 2, maxRows: 4 }" type="textarea"
          placeholder="Please input" />
      </div>
    </div>
  </header>

  <main>
    <div class="item">
      <div class="details">
        <h3>
          字符串包含“\n”前后引号包裹逗号结束
        </h3>
        <div style="cursor: pointer;" @click=clickCopy>
          <p v-for="(item, index) in formattedData" :key="index">{{ item }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {

  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}

.item {
  margin-top: 2rem;
  display: flex;
  position: relative;
}

.details {
  flex: 1;
  margin-left: 1rem;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;

  color: var(--color-text);
}

h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
  }

  i {
    top: calc(50% - 25px);
    left: -26px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }

  .item:before {
    content: ' ';
    /* border-left: 1px solid var(--color-border); */
    position: absolute;
    left: 0;
    bottom: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:after {
    content: ' ';
    /* border-left: 1px solid var(--color-border); */
    position: absolute;
    left: 0;
    top: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:first-of-type:before {
    display: none;
  }

  .item:last-of-type:after {
    display: none;
  }
}
</style>
