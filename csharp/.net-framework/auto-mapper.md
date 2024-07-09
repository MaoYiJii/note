# AutoMapper

#### MapTypeFromAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
    public class MapTypeFromAttribute : Attribute
    {
        public Type SourceType { get; private set; }
        public Type TypeConverterType { get; private set; }

        /// <summary>
        /// CreateMap(SourceType, TargetType)
        /// <para>TargetType 是添加這個標籤的類別</para>
        /// </summary>
        public MapTypeFromAttribute(Type sourceType)
        {
            SourceType = sourceType;
        }

        /// <summary>
        /// CreateMap(SourceType, TargetType).ConvertUsing(TypeConverterType)
        /// <para>TargetType 是添加這個標籤的類別</para>
        /// <para>TypeConverterType 需要繼承自 ITypeConverter&lt;SourceType, TargetType&gt;</para>
        /// </summary>
        public MapTypeFromAttribute(Type sourceType, Type typeConverterType)
        {
            SourceType = sourceType;
            TypeConverterType = typeConverterType;
        }
    }
}
```

#### MapTypeToAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Class)]
    public class MapTypeToAttribute : Attribute
    {
        public Type TargetType { get; private set; }
        public Type TypeConverterType { get; private set; }

        /// <summary>
        /// CreateMap(SourceType, TargetType)
        /// <para>SourceType 是添加這個標籤的類別</para>
        /// </summary>
        public MapTypeToAttribute(Type targetType)
        {
            TargetType = targetType;
        }

        /// <summary>
        /// CreateMap(SourceType, TargetType).ConvertUsing(TypeConverterType)
        /// <para>SourceType 是添加這個標籤的類別</para>
        /// <para>TypeConverterType 需要繼承自 ITypeConverter&lt;SourceType, TargetType&gt;</para>
        /// </summary>
        public MapTypeToAttribute(Type targetType, Type typeConverterType)
        {
            TargetType = targetType;
            TypeConverterType = typeConverterType;
        }
    }
}
```

#### MapMemberFromAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapMemberFromAttribute : Attribute
    {
        public string SourceMemberName { get; private set; }
        public Type ValueResolverType { get; private set; }
        public Type MemberValueResolverType { get; private set; }

        /// <summary>
        /// ForMember(TargetMemberName, options =&gt; options.MapFrom(SourceMemberName))
        /// <para>TargetMemberName 是添加這個標籤的成員名稱</para>
        /// </summary>
        public MapMemberFromAttribute(string sourceMemberName)
        {
            SourceMemberName = sourceMemberName;
        }

        /// <summary>
        /// ForMember(TargetMemberName, options =&gt; options.MapFrom(ValueResolverType))
        /// <para>TargetMemberName 是添加這個標籤的成員名稱</para>
        /// <para>ValueResolverType 需要繼承自 IValueResolver&lt;SourceType, TargetType, TargetMemberType&gt;</para>
        /// </summary>
        public MapMemberFromAttribute(Type valueResolverType)
        {
            ValueResolverType = valueResolverType;
        }

        /// <summary>
        /// ForMember(TargetMemberName, options =&gt; options.MapFrom(MemberValueResolverType, SourceMemberName))
        /// <para>TargetMemberName 是添加這個標籤的成員名稱</para>
        /// <para>MemberValueResolverType 需要繼承自 IMemberValueResolver&lt;SourceType, TargetType, SourceMemberType, TargetMemberType&gt;</para>
        /// </summary>
        public MapMemberFromAttribute(string sourceMemberName, Type memberValueResolverType)
        {
            SourceMemberName = sourceMemberName;
            MemberValueResolverType = memberValueResolverType;
        }
    }
}
```

#### MapMemberToAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapMemberToAttribute : Attribute
    {
        public string TargetMemberName { get; private set; }
        public Type MemberValueResolverType { get; private set; }

        public MapMemberToAttribute(string targetMemberName)
        {
            TargetMemberName = targetMemberName;
        }

        public MapMemberToAttribute(string targetMemberName, Type memberValueResolverType)
        {
            TargetMemberName = targetMemberName;
            MemberValueResolverType = memberValueResolverType;
        }
    }
}
```

#### MapMemberFromFuncAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapMemberFromFuncAttribute : Attribute
    {
        public string SourceMemberName { get; private set; }
        public Type MethodReflectedType { get; private set; }
        public string MethodName { get; private set; }

        public MapMemberFromFuncAttribute(Type methodReflectedType, string methodName)
        {
            MethodReflectedType = methodReflectedType;
            MethodName = methodName;
        }

        public MapMemberFromFuncAttribute(string sourceMemberName, Type methodReflectedType, string methodName)
        {
            SourceMemberName = sourceMemberName;
            MethodReflectedType = methodReflectedType;
            MethodName = methodName;
        }
    }
}
```

#### MapMemberToFuncAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapMemberToFuncAttribute : Attribute
    {
        public string TargetMemberName { get; private set; }
        public Type MethodReflectedType { get; private set; }
        public string MethodName { get; private set; }

        public MapMemberToFuncAttribute(string targetMemberName, Type methodReflectedType, string methodName)
        {
            TargetMemberName = targetMemberName;
            MethodReflectedType = methodReflectedType;
            MethodName = methodName;
        }
    }
}
```

#### MapSourceMemberIgnoreAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapSourceMemberIgnoreAttribute : Attribute
    {
    }
}
```

#### MapTargetMemberIgnoreAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapTargetMemberIgnoreAttribute : Attribute
    {
    }
}

```

#### MapMemberUseDestinationAttribute
``` cs
using System;
using System.Collections.Generic;
using System.Text;

namespace Helpers.Attributes
{
    [AttributeUsage(AttributeTargets.Property)]
    public class MapMemberUseDestinationAttribute : Attribute
    {
    }
}

```

#### ProfileExtensions
``` cs
using AutoMapper;
using Helpers.Attributes;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Linq.Expressions;
using System.Reflection;
using System.Text;

namespace Helpers.Extensions
{
    public static class ProfileExtensions
    {
        public static void CreateMaps<TServiceFactory>(this Profile profile, params Assembly[] assemblies)
        {
            foreach (Assembly assembly in assemblies)
            {
                Type[] mapTypes = assembly.GetTypes()
                    .Where(x => x.IsClass)
                    .Where(x => x.GetCustomAttributes<MapTypeFromAttribute>().Any() || x.GetCustomAttributes<MapTypeToAttribute>().Any())
                    .ToArray();
                foreach (Type mapType in mapTypes)
                {
                    var typeFromAttributes = mapType.GetCustomAttributes<MapTypeFromAttribute>();
                    if (typeFromAttributes != null && typeFromAttributes.Any())
                    {
                        foreach (MapTypeFromAttribute typeFromAttribute in typeFromAttributes)
                        {
                            CreateMap<TServiceFactory>(
                                profile,
                                typeFromAttribute.SourceType,
                                mapType,
                                typeFromAttribute.TypeConverterType);
                        }
                    }

                    var typeToAttributes = mapType.GetCustomAttributes<MapTypeToAttribute>();
                    if (typeToAttributes != null && typeToAttributes.Any())
                    {
                        foreach (MapTypeToAttribute typeToAttribute in typeToAttributes)
                        {
                            if (assemblies.Contains(mapType.Assembly) && typeToAttribute.TargetType.GetCustomAttributes<MapTypeFromAttribute>().Any())
                            {
                                typeFromAttributes = typeToAttribute.TargetType.GetCustomAttributes<MapTypeFromAttribute>();
                                if (typeFromAttributes != null && typeFromAttributes.Any(x => x.SourceType == mapType))
                                {
                                    // 同時設定 MapTypeFrom 與 MapTypeTo 時，忽略 MapTypeTo
                                    continue;
                                }
                            }
                            CreateMap<TServiceFactory>(
                                profile,
                                mapType,
                                typeToAttribute.TargetType,
                                typeToAttribute.TypeConverterType);
                        }
                    }
                }
            }
        }

        private static void CreateMap<TServiceFactory>(Profile profile, Type sourceType, Type targetType, Type typeConverterType = null)
        {
            var mapping = typeof(Profile)
                .FindMethod("CreateMap", new[] { "TSource", "TDestination" })
                .MakeGenericMethod(sourceType, targetType)
                .Invoke(profile, null);
            Type mappingType = typeof(IMappingExpression<,>)
                .MakeGenericType(sourceType, targetType);

            if (typeConverterType != null)
            {
                Type typeConverterInterface = typeConverterType.GetInterface("ITypeConverter`2");
                if (typeConverterInterface == null)
                {
                    throw new Exception($"{typeConverterType.Name} 必須繼承自 ITypeConverter<{sourceType.Name}, {targetType.Name}>");
                }
                Type[] typeConverterGenerics = typeConverterInterface.GetGenericArguments();
                if (typeConverterGenerics[0] != sourceType || typeConverterGenerics[1] != targetType)
                {
                    throw new Exception($"{typeConverterType.Name} 必須繼承自 ITypeConverter<{sourceType.Name}, {targetType.Name}>");
                }

                // mapping.ConvertUsing<typeConverterType>();
                mappingType
                    .FindMethod("ConvertUsing", new[] { "TTypeConverter" })
                    .MakeGenericMethod(typeConverterType)
                    .Invoke(mapping, null);

                // 有 TypeConverter 就忽略後續對 MapMember 標籤的處理
                return;
            }

            PropertyInfo[] sourceProperties = sourceType.GetProperties(BindingFlags.Public | BindingFlags.Instance);
            foreach (PropertyInfo sourceProperty in sourceProperties)
            {
                if (sourceProperty.GetCustomAttribute<MapSourceMemberIgnoreAttribute>() != null)
                {
                    Action<ISourceMemberConfigurationExpression> memberOptions = options => options.DoNotValidate();
                    mappingType
                        .FindMethod("ForSourceMember", null, new[] { "String", "Action`1" })
                        .Invoke(mapping, new object[] { sourceProperty.Name, memberOptions });
                }
                else if (sourceProperty.GetCustomAttribute<MapMemberToAttribute>() is MapMemberToAttribute memberToAttribute)
                {
                    PropertyInfo targetProperty = targetType.GetProperty(memberToAttribute.TargetMemberName);
                    if (sourceProperty == null)
                    {
                        throw new Exception($"{targetType.Name} 找不到名稱為 {memberToAttribute.TargetMemberName} 的成員");
                    }
                    if (memberToAttribute.MemberValueResolverType != null)
                    {
                        Type memberValueResolverInterface = memberToAttribute.MemberValueResolverType.GetInterface("IMemberValueResolver`4");
                        if (memberValueResolverInterface == null)
                        {
                            throw new Exception($"{memberToAttribute.MemberValueResolverType.Name} 必須繼承自 IMemberValueResolver<{sourceType.Name}, {targetType.Name}, {sourceProperty.PropertyType.Name}, {targetProperty.PropertyType.Name}>");
                        }
                        Type[] memberValueResolverGenerics = memberValueResolverInterface.GetGenericArguments();
                        if (memberValueResolverGenerics[0] != sourceType || memberValueResolverGenerics[1] != targetType || !memberValueResolverGenerics[2].IsAssignableFrom(sourceProperty.PropertyType) || !targetProperty.PropertyType.IsAssignableFrom(memberValueResolverGenerics[3]))
                        {
                            throw new Exception($"{memberToAttribute.MemberValueResolverType.Name} 必須繼承自 IMemberValueResolver<{sourceType.Name}, {targetType.Name}, {sourceProperty.PropertyType.Name}, {targetProperty.PropertyType.Name}>");
                        }

                        MappingForMember(
                            mapping,
                            targetProperty,
                            typeof(IMemberConfigurationExpression<,,>)
                                .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                                .FindMethod("MapFrom", new[] { "TValueResolver", "TSourceMember" }, new[] { typeof(string) })
                                .MakeGenericMethod(memberToAttribute.MemberValueResolverType, sourceProperty.PropertyType),
                            sourceProperty.Name);
                    }
                    else
                    {
                        MappingForMember(
                            mapping,
                            targetProperty,
                            typeof(IMemberConfigurationExpression<,,>)
                                .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                                .FindMethod("MapFrom", null, new[] { typeof(string) }),
                            sourceProperty.Name);
                    }
                }
                else if (sourceProperty.GetCustomAttribute<MapMemberToFuncAttribute>() is MapMemberToFuncAttribute memberToFuncAttribute)
                {
                    PropertyInfo targetProperty = targetType.GetProperty(memberToFuncAttribute.TargetMemberName);
                    if (sourceProperty == null)
                    {
                        throw new Exception($"{targetType.Name} 找不到名稱為 {memberToFuncAttribute.TargetMemberName} 的成員");
                    }

                    MethodInfo method = memberToFuncAttribute.MethodReflectedType.GetMethod(memberToFuncAttribute.MethodName);
                    if (method == null)
                    {
                        throw new Exception($"{memberToFuncAttribute.MethodReflectedType.Name} 找不到名稱為 {memberToFuncAttribute.MethodName} 的方法");
                    }

                    MappingForMemberFunc<TServiceFactory>(
                        mapping,
                        sourceProperty,
                        targetProperty,
                        method);
                }
            }

            PropertyInfo[] targetProperties = targetType.GetProperties(BindingFlags.Public | BindingFlags.Instance);
            foreach (PropertyInfo targetProperty in targetProperties)
            {
                if (targetProperty.GetCustomAttribute<MapTargetMemberIgnoreAttribute>() != null)
                {
                    MappingForMember(
                        mapping,
                        targetProperty,
                        typeof(IMemberConfigurationExpression<,,>)
                            .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                            .FindMethod("Ignore", null),
                        null);
                }
                else if (targetProperty.GetCustomAttribute<MapMemberUseDestinationAttribute>() != null)
                {
                    MappingForMember(
                        mapping,
                        targetProperty,
                        typeof(IMemberConfigurationExpression<,,>)
                            .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                            .FindMethod("UseDestinationValue", null),
                        null);
                }
                else if (targetProperty.GetCustomAttribute<MapMemberFromAttribute>() is MapMemberFromAttribute memberFromAttribute)
                {
                    PropertyInfo sourceProperty = null;
                    if (string.IsNullOrEmpty(memberFromAttribute.SourceMemberName) == false)
                    {
                        sourceProperty = sourceType.GetProperty(memberFromAttribute.SourceMemberName);
                        if (sourceProperty == null)
                        {
                            throw new Exception($"{sourceType.Name} 找不到名稱為 {memberFromAttribute.SourceMemberName} 的成員");
                        }
                    }
                    if (memberFromAttribute.ValueResolverType != null)
                    {
                        Type valueResolverInterface = memberFromAttribute.ValueResolverType.GetInterface("IValueResolver`3");
                        if (valueResolverInterface == null)
                        {
                            throw new Exception($"{memberFromAttribute.ValueResolverType.Name} 必須繼承自 IValueResolver<{sourceType.Name}, {targetType.Name}, {targetProperty.PropertyType.Name}>");
                        }
                        Type[] valueResolverGenerics = valueResolverInterface.GetGenericArguments();
                        if (valueResolverGenerics[0] != sourceType || valueResolverGenerics[1] != targetType || !targetProperty.PropertyType.IsAssignableFrom(valueResolverGenerics[2]))
                        {
                            throw new Exception($"{memberFromAttribute.ValueResolverType.Name} 必須繼承自 IValueResolver<{sourceType.Name}, {targetType.Name}, {targetProperty.PropertyType.Name}>");
                        }

                        MappingForMember(
                            mapping,
                            targetProperty,
                            typeof(IMemberConfigurationExpression<,,>)
                                .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                                .FindMethod("MapFrom", new[] { "TValueResolver" })
                                .MakeGenericMethod(memberFromAttribute.ValueResolverType),
                            null);
                    }
                    else if (memberFromAttribute.MemberValueResolverType != null)
                    {
                        Type memberValueResolverInterface = memberFromAttribute.MemberValueResolverType.GetInterface("IMemberValueResolver`4");
                        if (memberValueResolverInterface == null)
                        {
                            throw new Exception($"{memberFromAttribute.MemberValueResolverType.Name} 必須繼承自 IMemberValueResolver<{sourceType.Name}, {targetType.Name}, {sourceProperty.PropertyType.Name}, {targetProperty.PropertyType.Name}>");
                        }
                        Type[] memberValueResolverGenerics = memberValueResolverInterface.GetGenericArguments();
                        if (memberValueResolverGenerics[0] != sourceType || memberValueResolverGenerics[1] != targetType || !memberValueResolverGenerics[2].IsAssignableFrom(sourceProperty.PropertyType) || !targetProperty.PropertyType.IsAssignableFrom(memberValueResolverGenerics[3]))
                        {
                            throw new Exception($"{memberFromAttribute.MemberValueResolverType.Name} 必須繼承自 IMemberValueResolver<{sourceType.Name}, {targetType.Name}, {sourceProperty.PropertyType.Name}, {targetProperty.PropertyType.Name}>");
                        }

                        MappingForMember(
                            mapping,
                            targetProperty,
                            typeof(IMemberConfigurationExpression<,,>)
                                .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                                .FindMethod("MapFrom", new[] { "TValueResolver", "TSourceMember" }, new[] { typeof(string) })
                                .MakeGenericMethod(memberFromAttribute.MemberValueResolverType, sourceProperty.PropertyType),
                            sourceProperty.Name);
                    }
                    else
                    {
                        MappingForMember(
                            mapping,
                            targetProperty,
                            typeof(IMemberConfigurationExpression<,,>)
                                .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                                .FindMethod("MapFrom", null, new[] { typeof(string) }),
                            sourceProperty.Name);
                    }
                }
                else if (targetProperty.GetCustomAttribute<MapMemberFromFuncAttribute>() is MapMemberFromFuncAttribute memberFromFuncAttribute)
                {
                    PropertyInfo sourceProperty = null;
                    if (string.IsNullOrEmpty(memberFromFuncAttribute.SourceMemberName) == false)
                    {
                        sourceProperty = sourceType.GetProperty(memberFromFuncAttribute.SourceMemberName);
                        if (sourceProperty == null)
                        {
                            throw new Exception($"{sourceType.Name} 找不到名稱為 {memberFromFuncAttribute.SourceMemberName} 的成員");
                        }
                    }

                    MethodInfo method = memberFromFuncAttribute.MethodReflectedType.GetMethod(memberFromFuncAttribute.MethodName);
                    if (method == null)
                    {
                        throw new Exception($"{memberFromFuncAttribute.MethodReflectedType.Name} 找不到名稱為 {memberFromFuncAttribute.MethodName} 的方法");
                    }

                    if (sourceProperty == null)
                    {
                        MappingForMemberFunc<TServiceFactory>(
                            mapping,
                            targetProperty,
                            method);
                    }
                    else
                    {
                        MappingForMemberFunc<TServiceFactory>(
                            mapping,
                            sourceProperty,
                            targetProperty,
                            method);
                    }
                }
            }
        }

        private static void MappingForMember(object mapping, PropertyInfo targetProperty, MethodInfo mapFrom, params object[] mapFromParameters)
        {
            Type mappingType = mapping.GetType();
            var genericArguments = mappingType.GetGenericArguments();
            Type sourceType = genericArguments[0];
            Type targetType = genericArguments[1];

            var destination = Expression.Parameter(targetType, "destination");
            var options = Expression.Parameter(
                typeof(IMemberConfigurationExpression<,,>)
                    .MakeGenericType(sourceType, targetType, targetProperty.PropertyType),
                "options");
            mappingType
                .FindMethod("ForMember", new[] { "TMember" }, new[] { "Expression`1", "Action`1" })
                .MakeGenericMethod(targetProperty.PropertyType)
                .Invoke(
                    mapping,
                    new object[]
                    {
                        Expression.Lambda(
                            typeof(Func<,>)
                                .MakeGenericType(targetType, targetProperty.PropertyType),
                            Expression.Property(destination, targetProperty.Name),
                            destination),
                        Expression.Lambda(
                            typeof(Action<>)
                                .MakeGenericType(
                                    typeof(IMemberConfigurationExpression<,,>)
                                        .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)),
                            Expression.Call(
                                options,
                                mapFrom,
                                mapFromParameters?.Select(x => Expression.Constant(x)).ToArray()),
                            options)
                            .Compile()
                    });
        }

        private static void MappingForMemberFunc<TServiceFactory>(object mapping, PropertyInfo targetProperty, MethodInfo method)
        {
            Type mappingType = mapping.GetType();
            Type[] genericArguments = mappingType.GetGenericArguments();
            Type sourceType = genericArguments[0];
            Type targetType = genericArguments[1];

            var source = Expression.Parameter(sourceType, "source");
            var destination = Expression.Parameter(targetType, "destination");
            var destMember = Expression.Parameter(targetProperty.PropertyType, "destMember");

            ParameterInfo[] parameters = method.GetParameters();

            Expression[] invokeParameters =
                parameters.Length == 1 ? new[] { source } :
                parameters.Length == 2 ? new[] { source, destination } :
                parameters.Length == 3 ? new[] { source, destination, destMember } :
                null;

            var func = Expression.Lambda(
                typeof(Func<,,,>)
                    .MakeGenericType(sourceType, targetType, targetProperty.PropertyType, targetProperty.PropertyType),
                Expression.Call(
                    Expression.Call(
                        typeof(TServiceFactory)
                            .FindMethod("CreateInstance", new[] { "T" })
                            .MakeGenericMethod(method.ReflectedType)),
                    method,
                    invokeParameters),
                source, destination, destMember)
                .Compile();

            MappingForMember(
                mapping,
                targetProperty,
                typeof(IMemberConfigurationExpression<,,>)
                    .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                    .FindMethod("MapFrom", new[] { "TResult" }, new[] { "Func`4" })
                    .MakeGenericMethod(targetProperty.PropertyType),
                func);
        }

        private static void MappingForMemberFunc<TServiceFactory>(object mapping, PropertyInfo sourceProperty, PropertyInfo targetProperty, MethodInfo method)
        {
            Type mappingType = mapping.GetType();
            Type[] genericArguments = mappingType.GetGenericArguments();
            Type sourceType = genericArguments[0];
            Type targetType = genericArguments[1];

            var source = Expression.Parameter(sourceType, "source");
            var destination = Expression.Parameter(targetType, "destination");
            var sourceMember = Expression.Property(source, sourceProperty);
            var destMember = Expression.Parameter(targetProperty.PropertyType, "destMember");

            ParameterInfo[] parameters = method.GetParameters();

            Expression[] invokeParameters =
                parameters.Length == 1 ? new[] { source } :
                parameters.Length == 2 ? new[] { source, destination } :
                parameters.Length == 3 ? new[] { source, destination, destMember } :
                parameters.Length == 4 ? new Expression[] { source, destination, sourceMember, destMember } :
                null;

            var func = Expression.Lambda(
                typeof(Func<,,,>)
                    .MakeGenericType(sourceType, targetType, targetProperty.PropertyType, targetProperty.PropertyType),
                Expression.Call(
                    Expression.Call(
                        typeof(TServiceFactory)
                            .FindMethod("CreateInstance", new[] { "T" })
                            .MakeGenericMethod(method.ReflectedType)),
                    method,
                    invokeParameters),
                source, destination, destMember)
                .Compile();

            MappingForMember(
                mapping,
                targetProperty,
                typeof(IMemberConfigurationExpression<,,>)
                    .MakeGenericType(sourceType, targetType, targetProperty.PropertyType)
                    .FindMethod("MapFrom", new[] { "TResult" }, new[] { "Func`4" })
                    .MakeGenericMethod(targetProperty.PropertyType),
                func);
        }
    }
}
```