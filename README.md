# mybatis-generator
mybatis-generator 基于velocity mybatis-puls实现模版代码 自动生成


## 使用说明

修改MpGenerator配置设置，运行main

## 模版代码 Controller

```java
	package ${package.Controller};
#if(${superControllerClassPackage})
import ${superControllerClassPackage};
#end
import ${package.Entity}.${entity};
import ${package.Service}.${table.serviceName};
import org.springframework.beans.factory.annotation.Autowired;
import org.apache.shiro.authz.annotation.RequiresPermissions;
import org.springframework.web.bind.annotation.*;
import io.swagger.annotations.*;
import springfox.documentation.annotations.ApiIgnore;

/**
 * <li>文件名称: ${table.comment} 前端控制器</li>
 * <li>文件描述: ${table.comment} 前端控制器</li>
 * <li>版权所有: 版权所有© 2005-2017</li>
 * <li>公 司: xxxxx股份有限公司</li>
 * <li>内容摘要: 无</li>
 * <li>其他说明:无</li>
 * <li>完成日期： ${date}</li>
 * <li>修改记录: 无</li>
 * @version 版本号
 * @author ${author}
 */
@RestController
@RequestMapping("#if(${package.ModuleName})/${package.ModuleName}#end/${table.entityPath}")
#if(${superControllerClass})
@Api(value = "${table.comment}接口", description = "用作${table.comment}演示")
public class ${table.controllerName} extends${superControllerClass}<${entity}> {
#else
@Api(value = "${table.comment}接口", description = "用作${table.comment}演示")
public class ${table.controllerName} {
#end

#foreach($field in ${table.fields})
    #if(${field.keyFlag})
        #set($keyPropertyName=${field.propertyName})
        #set($keyPropertyAttr=${field.propertyType})
    #end
#end

#set ($servicePropertyName = $table.serviceName.substring(0,1).toLowerCase() + $table.serviceName.substring(1,$table.serviceName.length()))
#set ($classname = $entity.substring(0,1).toLowerCase() + $entity.substring(1,$entity.length()))
    @Autowired
    private ${table.serviceName} ${servicePropertyName};

    @Autowired
    private MessageSource messageSource;

##/**
## * 列表
## */
##@GetMapping("/list")
##@RequiresPermissions("#if(${package.ModuleName})#end${table.entityPath}:list")
##@ApiOperation(value = "${table.comment}", notes = "获取${table.comment}列表")
##@ApiImplicitParams({
##        @ApiImplicitParam(name = "currentPage", value = "当前页码", paramType = "query"),
##        @ApiImplicitParam(name = "pageSize", value = "每页条数", paramType = "query")
##})
##public R list(@ApiIgnore BaseQuery baseQuery){
##        //查询列表数据
##        Page page=new Page(baseQuery.getCurrentPage(),baseQuery.getPageSize());
##        Page pageList=${servicePropertyName}.selectPage(page,new EntityWrapper<${entity}>());
##        if(CollectionUtils.isEmpty(pageList.getRecords())){
##        return R.notFound();
##        }
##        return R.fillPageData(pageList);
##        }
##
##
##/**
## * 信息
## */
##@GetMapping("/info/{${keyPropertyName}}")
##@RequiresPermissions("#if(${package.ModuleName})#end${table.entityPath}:info")
##@ApiOperation(value = "${table.comment}", notes = "获取${table.comment}详情信息")
##public R info(@PathVariable("${keyPropertyName}") ${keyPropertyAttr} ${keyPropertyName}){
##    ${entity} ${classname} = ${servicePropertyName}.selectById(${keyPropertyName});
##        if(${classname} ==null){
##        return R.notFound();
##        }
##        return R.fillSingleData(${classname});
##        }
##
##/**
## * 保存
## */
##@PostMapping("/save")
##@RequiresPermissions("#if(${package.ModuleName})#end${table.entityPath}:save")
##@ApiOperation(value = "${table.comment}", notes = "保存${table.comment}信息")
##public R save(@RequestBody ${entity} ${classname}){
##        boolean retFlag= ${servicePropertyName}.insert(${classname});
##        if(!retFlag){
##        return R.error(messageSource.getMessage("10001",null,Locale.CHINESE));
##        }
##        return R.ok();
##        }
##
##/**
## * 修改
## */
##@PostMapping("/update")
##@RequiresPermissions("#if(${package.ModuleName})#end${table.entityPath}:update")
##@ApiOperation(value = "${table.comment}", notes = "更新${table.comment}信息")
##public R update(@RequestBody ${entity} ${classname}){
##        boolean retFlag= ${servicePropertyName}.updateById(${classname});
##        if(!retFlag){
##        return R.error(messageSource.getMessage("10001",null,Locale.CHINESE));
##        }
##        return R.ok();
##        }
##
##/**
## * 删除
## */
##@PostMapping("/delete/{${keyPropertyName}}")
##@RequiresPermissions("#if(${package.ModuleName})#end${table.entityPath}:delete")
##@ApiOperation(value = "${table.comment}", notes = "删除${table.comment}信息")
##public R delete(@PathVariable("${keyPropertyName}") ${keyPropertyAttr} ${keyPropertyName}){
##        boolean retFlag= ${servicePropertyName}.deleteById(${keyPropertyName});
##        if(!retFlag){
##        return R.error(messageSource.getMessage("10001",null,Locale.CHINESE));
##        }
##        return R.ok();
##        }
    }

```


### 其他

> * Entity.java.vm
> * mapper.java.vm
> * mapper.xml.vm
> * Service.java.vm
> * ServiceImpl.java.vm
