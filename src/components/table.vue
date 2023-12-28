<template>
  <div id="app">
    <h2>省市区下拉级联选择器</h2>
  
    <label for="province">省：</label>
    <select id="province" v-model="selectedProvince" @change="onProvinceChange">
      <option value="">请选择省</option>
      <option v-for="(province, index) in provinces" :key="index" :value="province.code">{{ province.name }}</option>
    </select>
    
    <label for="city">市：</label>
    <select id="city" v-model="selectedCity" @change="onCityChange">
      <option value="">请选择市</option>
      <option v-for="(city,index) in cities" :key="index" :value="city.code">{{ city.name }}</option>
      <option v-if="!cityExists(selectedCity)" :value="selectedCity">{{ selectedCity }}</option>
    </select>
    
    <label for="district">区/县：</label>
    <select id="district" v-model="selectedDistrict">
      <option value="">请选择区/县</option>
      <option v-for="(district,index) in districts" :key="index" :value="district.code">{{ district.name }}</option>
      <option v-if="!districtExists(selectedDistrict)" :value="selectedDistrict">{{ selectedDistrict }}</option>
    </select>
  </div>
</template>
<script>
export default {
  data(){
    return {
      selectedProvince: '',
      selectedCity: '',
      selectedDistrict: '',
      provinces: [],
      cities: [],
      districts: []
    }
  },
  mounted() {
    // 加载省级数据
    this.loadProvinces();
  },
  methods: {
    loadProvinces() {
      // 模拟异步加载省级数据
      setTimeout(() => {
        // 假设数据以JSON格式存储在 provincesData 变量中
        const provincesData = [
          { code: '1', name: '省1' },
          { code: '2', name: '省2' },
          { code: '3', name: '省3' }
        ];
        this.provinces = provincesData;
      }, 500);
    },
    onProvinceChange() {
      // 清空市级和区级选项
      this.selectedCity = '';
      this.selectedDistrict = '';
      
      if (this.selectedProvince) {
        // 模拟异步加载市级数据
        setTimeout(() => {
          // 假设数据以JSON格式存储在 citiesData 变量中
          const citiesData = [
            { code: '11', name: '市1' },
            { code: '12', name: '市2' },
            { code: '13', name: '市3' }
          ];
          this.cities = citiesData;
        }, 500);
      } else {
        this.cities = [];
      }
    },
    onCityChange() {
      // 清空区级选项
      this.selectedDistrict = '';
      
      if (this.selectedCity) {
        // 模拟异步加载区级数据
        setTimeout(() => {
          // 假设数据以JSON格式存储在 districtsData 变量中
          const districtsData = [
            { code: '111', name: '区1' },
            { code: '112', name: '区2' },
            { code: '113', name: '区3' }
          ];
          this.districts = districtsData;
        }, 500);
      } else {
        this.districts = [];
      }
    },
    cityExists(city) {
      // 检查输入的城市是否已存在
      return this.cities.some(c => c.name === city);
    },
    districtExists(district) {
      // 检查输入的区/县是否已存在
      return this.districts.some(d => d.name === district);
    }
  }
}
</script>
<style>

</style>