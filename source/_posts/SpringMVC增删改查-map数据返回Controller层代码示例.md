title: SpringMVC增删改查+map数据返回Controller层代码示例
author: 学亮
tags:
  - springmvc
categories: []
date: 2019-04-28 16:26:00
---

```
@RestController
@RequestMapping("/brand")
public class BrandController {
	@Reference
	private BrandService brandService;
	
	@RequestMapping("/findAll")
	public List<TbBrand> findAll(){
		return brandService.findAll();
	}
	
	/**
	 * 品牌分页查询
	 * @param page  当前页码
	 * @param size	当前页记录条数
	 * @return
	 */
	@RequestMapping("/findPage")
	public PageResult findPage(int page,int size){
		return brandService.findPage(page, size);
	}
	
	/**
	 * 增加品牌
	 * @param brand
	 * @return
	 */
	@RequestMapping("/add")
	public Result add(@RequestBody TbBrand brand){//@RequestBody接收页面传递过来的参数  非URL拼接
		try {
			brandService.add(brand);
			return new Result(true,"增加成功");
		} catch (Exception e) {
			e.printStackTrace();
			return new Result(false,"增加失败");
		}
	}
	
	/**
	 * 品牌修改  两个方法
	 * @param id
	 * @return
	 */
	@RequestMapping("/findOne")
	public TbBrand findOne(long id){
		return brandService.findOne(id);
	}
	@RequestMapping("/update")
	public Result update(@RequestBody TbBrand brand){
		try {
			brandService.update(brand);
			return new Result(true,"修改成功");
		} catch (Exception e) {
			e.printStackTrace();
			return new Result(false,"修改失败");
		}
	}
	
	/**
	 * 品牌删除
	 */
	@RequestMapping("/delete")
	public Result delete(long[] ids){
		try {
			brandService.delete(ids);
			return new Result(true,"删除成功");
		} catch (Exception e) {
			e.printStackTrace();
			return new Result(false,"删除失败");
		}
	}
	
	/**
	 * 品牌条件查询
	 */
	@RequestMapping("/search")
	public PageResult search(@RequestBody TbBrand brand,int page,int size){
		return brandService.findPage(brand, page, size);
	}
	
	@RequestMapping("/selectOptionList")
	public List<Map> selectOptionList(){
		return brandService.selectOptionList();
	}
}
```